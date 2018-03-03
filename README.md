# deps
cljs

(ns singlepage-app-om.core
  )

(enable-console-print!)

;; helper

(defn by-id [elem-id]
  (.getElementById js/document elem-id))

(println "Hello world!")
 ;; ADDED
 (defn foo [a b] (+ a b))

(def THREE js/THREE) 
 (def camera (THREE.PerspectiveCamera. 75 (/ (.-innerWidth js/window) 
                                  (.-innerHeight js/window)) 1 10000)) 
 (set! (.-z (.-position camera)) 1000) 
 (def scene (THREE.Scene.)) 
 (def geometry (THREE.CubeGeometry. 200 200 200)) 
 (def obj (js/Object.)) 
 (set! (.-color obj) 0xff0000) 
 (set! (.-wireframe obj) true) 
 (def material (THREE.MeshBasicMaterial. obj)) 
 (def mesh (THREE.Mesh. geometry material)) 
 (.add scene mesh) 
 (def renderer (THREE.WebGLRenderer.)) 
 (.setSize renderer (.-innerWidth js/window) (.-innerHeight js/window)) 
 (.appendChild (.-body js/document) (.-domElement renderer)) 
   
 (defn render [] 
   (set! (.-x (.-rotation mesh)) (+ (.-x (.-rotation mesh)) 0.01)) 
   (set! (.-y (.-rotation mesh)) (+ (.-y (.-rotation mesh)) 0.02)) 
   (.render renderer scene camera)) 
   
 (defn animate [] 
   (.requestAnimationFrame js/window animate) 
   (render)) 
   
;; (animate) 

(def vertshader "varying vec2 vUv; 
void main()
{
    vUv = uv;

    vec4 mvPosition = modelViewMatrix * vec4(position, 1.0 );
    gl_Position = projectionMatrix * mvPosition;
}")

(def fragshader "uniform float iGlobalTime;
uniform sampler2D iChannel0;
uniform sampler2D iChannel1;

varying vec2 vUv;
void main(void)
{
    //vec2 p = gl_FragCoord.xy / iResolution.xy;
    vec2 p = -1.0 + 2.0 *vUv;
    vec2 q = p - vec2(0.5, 0.5);

    q.x += sin(iGlobalTime* 0.6) * 0.2;
    q.y += cos(iGlobalTime* 0.4) * 0.3;

    float len = length(q);

    float a = atan(q.y, q.x) + iGlobalTime * 0.3;
    float b = atan(q.y, q.x) + iGlobalTime * 0.3;
    float r1 = 0.3 / len + iGlobalTime * 0.5;
    float r2 = 0.2 / len + iGlobalTime * 0.5;

    float m = (1.0 + sin(iGlobalTime * 0.5)) / 2.0;
    vec4 tex1 = texture2D(iChannel0, vec2(a + 0.1 / len, r1 ));
    vec4 tex2 = texture2D(iChannel1, vec2(b + 0.1 / len, r2 ));
    vec3 col = vec3(mix(tex1, tex2, m));
    gl_FragColor = vec4(col * len * 1.5, 1.0);
}")

(println vertshader)
(println fragshader)

(def width (.-innerWidth js/window))
(def height (.-innerHeight js/window))
(def aspect (/ width height))
(def fov 65)
(def clipPlaneNear 0.1)
(def clipPlaneFar 1000)
(def clearColor 0x221f26)
(def clearAlpha 1.0)
(def clock THREE.Clock.)

(def tuniform1
 (js-obj "iGlobalTime" 666))

(println tuniform1)


(println (.-iGlobalTime tuniform1))

(def tuniform {
:iGlobalTime {:type "f" :value 0.1}
:iChannel0 {:type "t" :value (THREE.ImageUtils.loadTexture. "images/tex07.jpg" )}
:iChannel1 {:type "t" :value (THREE.ImageUtils.loadTexture. "images/tex03.jpg" )}
})

;;todo(set! (.-wrapS ()))

(def scene (THREE.Scene.))

(def camera (THREE.PerspectiveCamera. fov aspect clipPlaneNear clipPlaneFar))

(def renderer (THREE.WebGLRenderer. {:antialias true}))

(.setSize renderer width height)
(.setClearColor renderer (THREE.Color. clearColor clearAlpha))
(def mat (THREE.ShaderMaterial. {
:uniforms tuniform
:vertexShader vertshader
:fragShader fragshader
:side (.-DoubleSide THREE)}))

(set! (.-value (.-iGlobalTime tuniform)) (+ (.-value (.-iGlobalTime tuniform)) (.getDelta clock)))

;;(def mat (THREE.Mesh. (THREE.CubeGeometry. 700 394 1 1) mat))


index.html

<!DOCTYPE html><html>  <head>    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">  </head>  <body>    <div class="container">      <div id="alert"></div>      <div id="app"></div>    </div>    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/90/three.js" type="text/javascript"></script>    <script src="js/out/goog/base.js" type="text/javascript"></script>    <script src="js/app.js" type="text/javascript"></script>    <script type="text/javascript">goog.require("singlepage_app_om.core");</script>  </body></html>
