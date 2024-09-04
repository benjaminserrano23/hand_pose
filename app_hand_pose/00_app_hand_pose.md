# Clasificador de Vocales en Lenguaje de Señas


<div id="p5-sketch">

<div id="canvas-container">

</div>

</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.9.0/p5.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.9.0/addons/p5.dom.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.9.0/addons/p5.sound.min.js"></script>
<script src="https://unpkg.com/ml5@0.12.2/dist/ml5.min.js"></script>
<script>
let video;
let classifier;
let modelLoaded = 'https://teachablemachine.withgoogle.com/models/Eq-HEZCYs/';
let label = 'Resultado...';
let confidence = 'Confianza...';
&#10;function preload(){
  classifier = ml5.imageClassifier(modelLoaded);
}
&#10;function setup() {
  createCanvas(640, 520);
  // Create the video
  video = createCapture(VIDEO);
  video.size(640, 480);
  video.hide();
&#10;  // Start classifying every 1 second
  setInterval(classifyVideo, 1000);
}
&#10;function classifyVideo() {
  classifier.classify(video, gotResults);
}
&#10;function draw() {
  background(0);
  &#10;  // Center the video on the canvas
  let x = (width - video.width) / 2;
  let y = (height - video.height) / 2;
  image(video, x, y);
&#10;  // Draw the label and confidence
  textSize(30);
  textAlign(CENTER, CENTER);
  fill(255);
  text(label, width / 4, height - 16);
  text(confidence, width / 1.4, height - 16);
}
&#10;function gotResults(error, results) {
  if (error) {
    console.error(error);
    return;
  }
&#10;  // Update the label and confidence
  label = results[0].label;
  confidence = nf(results[0].confidence * 100, 0, 2) + '%';
&#10;  // Only display if the label is one of the vowels
  if (['letra A', 'letra E', 'letra I', 'letra O', 'letra U'].includes(label)) {
    label = `${label}`;
  } else {
    label = 'No es una vocal';
  }
}
</script>
