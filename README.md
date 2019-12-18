# CP2_Final
A webcam that is made out of letters and punctuations
https://editor.p5js.org/kaitomiyake/full/aykRpaA_k
let camera;
let sampleSize = 6;
let camWidth = 800;
let camHeight = 420;
let proportion = camWidth / camHeight;
let fontSlider;
let fontBold;
let letters = " .,:;xX&#";
letters = letters.split("");
let spacer = "";

function preload() {
}

function setup() {
	camera = createCapture(VIDEO);
	camera.size(camWidth, camHeight);
	camera.hide();

	if (windowHeight < windowWidth) {
		createCanvas(round(windowHeight * proportion), windowHeight);
	} else {
		createCanvas(windowWidth, round(windowHeight * (1 / proportion)));
	}
	//create spacers to ensure the image is not skewed
	for (let i = 0; i < sampleSize / 10; i++) {
		spacer += " ";
	}
	slider = createSlider(6, 100, 6);
	slider.position((width/6)-(slider.width/2), height-40);
	slider.input(()=>{sampleSize = slider.value();});
	textSize(8);
	textFont('Courier New');
}

function draw() {
	//hook this up so it changes when you move the slider
	ascii();
    translate(camera.width, 0);
    scale(-1, 1);
}

// 255*3 = 765
let maxColor = 765;

function ascii() {
	let result = "";
	// let pixelSize = slider.value();
	camera.loadPixels();
	background(255);

	for (let y = 0; y < camera.height; y += sampleSize) {
		for (let x = 0; x < camera.width; x += sampleSize) {
			const i = ((y * camera.width) + x) * 4;
			const r = camera.pixels[i];
			const g = camera.pixels[i + 1];
			const b = camera.pixels[i + 2];
			let letterIndex = round(map(r+g+b,0,maxColor, letters.length - 1, 0));
			// text(
          result += letters[letterIndex]// + spacer;
		}
		//add a return to the end of each line
		result += "\n";
	}
	text(result, 0, 0);
    textLeading(7);
}
