<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>P5.js Game with Clickable Zones</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.min.js"></script>
  <style>
    body {
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f0f0f0;
      overflow: hidden;
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body id="sketch-holder">
  <script>
    let bgImage;
    let message = ""; // Message to display when zones are clicked
    let score = 0; // Score variable
    let interactions = 0; // Interactions counter
    const totalZones = 6; // Total number of interactive zones

    // Define interactive zones
    const zones = [
      { x: 200, y: 200, w: 100, h: 100, name: "phone", message: "The owner is studying, phone cannot be answered.", score: true },
      { x: 500, y: 200, w: 150, h: 100, name: "computer", message: "The computer is unlocked.", score: true },
      { x: 300, y: 500, w: 120, h: 80, name: "book", message: "The book is open to a difficult chapter.", score: true },
      { x: 400, y: 100, w: 70, h: 70, name: "pen", message: "This pen is out of ink.", score: true },
      { x: 600, y: 300, w: 100, h: 150, name: "flower vase", message: "The flower vase has a beautiful arrangement.", score: false },
      { x: 700, y: 400, w: 80, h: 80, name: "hair", message: "Some hair is on the table.", score: false },
      // Add more zones as needed, setting score: false for non-scoring objects
    ];

    function preload() {
      bgImage = loadImage('afro.png'); // Background image
    }

    function setup() {
      let cnv = createCanvas(windowWidth, windowHeight);
      cnv.parent('sketch-holder');
    }

    function draw() {
      background(bgImage);

      if (interactions < totalZones) {
        // Draw interactive zones
        zones.forEach(zone => {
          noFill(); // Make zones visible for debugging
          stroke(255, 0, 0);
          rect(zone.x, zone.y, zone.w, zone.h);
        });

        // Display the clicked message and object name
        fill(0);
        textSize(20);
        textAlign(CENTER, CENTER);
        text(message, width / 2, height - 50); // Display at the bottom of the canvas

        // Display the score
        fill(0);
        textSize(20);
        textAlign(LEFT, TOP);
        text("Score: " + score, 10, 10);

        // Show mouse coordinates
        displayCoordinates();
      } else {
        // Display ending screen
        background(200);
        (fill(0));
        textSize(32);
        textAlign(CENTER, CENTER);
        text("Game Over! Final Score: " + score, width / 2, height / 2);
      }
    }

    function mousePressed() {
      if (interactions < totalZones) {
        // Check if mouse is inside any interactive zone
        zones.forEach(zone => {
          if (
            mouseX > zone.x &&
            mouseX < zone.x + zone.w &&
            mouseY > zone.y &&
            mouseY < zone.y + zone.h
          ) {
            message = zone.name + ": " + zone.message; // Set the message when a zone is clicked
            if (zone.score) {
              score += 10; // Increase the score only for correct choices
            }
            interactions++; // Increase the interaction count

            // Check if all zones have been clicked
            if (interactions >= totalZones) {
              message = ""; // Clear the message
            }
          }
        });
      }
    }

    function displayCoordinates() {
      fill(255);
      textSize(16);
      textAlign(LEFT, BOTTOM);
      text(`X: ${mouseX}, Y: ${mouseY}`, 10, height - 10);
    }
  </script>
</body>
</html>
