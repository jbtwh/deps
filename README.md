# deps
cljs
<code>
(ns singlepage-app-om.core  )
(enable-console-print!)
;; helper
(defn by-id [elem-id]  (.getElementById js/document elem-id))
(println "Hello world!") ;; ADDED (defn foo [a b] (+ a b))
(def THREE js/THREE)  (def camera (THREE.PerspectiveCamera. 75 (/ (.-innerWidth js/window)                                   (.-innerHeight js/window)) 1 10000))  (set! (.-z (.-position camera)) 1000)  (def scene (THREE.Scene.))  (def geometry (THREE.CubeGeometry. 200 200 200))  (def obj (js/Object.))  (set! (.-color obj) 0xff0000)  (set! (.-wireframe obj) true)  (def material (THREE.MeshBasicMaterial. obj))  (def mesh (THREE.Mesh. geometry material))  (.add scene mesh)  (def renderer (THREE.WebGLRenderer.))  (.setSize renderer (.-innerWidth js/window) (.-innerHeight js/window))  (.appendChild (.-body js/document) (.-domElement renderer))     (defn render []    (set! (.-x (.-rotation mesh)) (+ (.-x (.-rotation mesh)) 0.01))    (set! (.-y (.-rotation mesh)) (+ (.-y (.-rotation mesh)) 0.02))    (.render renderer scene camera))     (defn animate []    (.requestAnimationFrame js/window animate)    (render))     (animate) 
</code>

index.html
<code>
<!DOCTYPE html><html>  <head>    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">  </head>  <body>    <div class="container">      <div id="alert"></div>      <div id="app"></div>    </div>    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/90/three.js" type="text/javascript"></script>    <script src="js/out/goog/base.js" type="text/javascript"></script>    <script src="js/app.js" type="text/javascript"></script>    <script type="text/javascript">goog.require("singlepage_app_om.core");</script>  </body></html>
</code>
