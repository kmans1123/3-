3차원 지형생성과 물체의 인터랙션를 구현한 컴퓨터 그래픽스 코딩

요약: 컴퓨터 그래픽스는 게임, 애니메이션, 영화, 광고 등 다양한 분야에서 활용되고 있다. 컴퓨터 그래픽스를 구현하고 웹 브라우저에서 동작하는 스크립트 언어로, 웹 페이지를 동적으로 만들어 3D 기초 그래픽에 대해 경험 해보고 학습한다.

1. 서론 : 4차 산업혁명 시대에 따라 인공지능이 보편화 되고 활용할 수 있는 분야가 많아짐에 따라 3D 지형을 생성하고 그에 대하여 설계한 시나리오를 chat gpt와 자바스크립트 혹은 3D 지형에 대한 예제와 대화를 통해 조금 더 완성도 높은 3D 지형 그래픽을 완성할 수 있다. 또는 오타나 문법적 오류를 chat gpt를 활용하여 수정을 조금 더 편리하고 빠르게 그리고 정확하게 제공 받는다. 자바스크립트는 웹 페이지를 동적으로 만들기 위한 프로그래밍 언어다. HTML과 CSS로 만들어진 정적인 웹 페이지를 동적으로 만들어주며, 사용자와 상호작용할 수 있는 기능을 제공다. 

2. 본론 : 아래 코드는 p5.js 라이브러리를 사용하여 3D 지형과 구를 그리는 예제다.

    setup() 함수에서는 캔버스를 생성하고, 지형 배열을 초기화한다.
    
    preload() 함수에서는 배경 이미지와 구 이미지를 불러온다.
    
    draw() 함수에서는 지형 배열을 노이즈 함수를 사용하여 생성하고, 배경 이미지와 지형을 그린다.

    HTML5의 Canvas 요소를 활용하여 자바스크립트를 이용하여 그림을 그리고 애니메이션을 만들 수 있는 기능을 제공한다.

    WEBGL을 통해 3D로 만들어 지형에 대한 색을 조절 하고 sphere을 이용해서 구를 하나 만든다. 만든 구에 대해 불러왔던 사진을 적용시키고 rotateX(mouseY/10);, rotateY(mouseX/10);로     마우스를 따라 움직이는 구를 만든다.
 
    마우스가 움직일 때 잔상이 남는 것을 방지하기 위해서는 background() 함수를 사용하여 매 프레임마다 캔버스를 지운다. background() 함수는 캔버스의 배경색을 지정하는 함수로, 매 프     레임마다 호출되어 캔버스를 지우고 새로운 프레임을 그린다.
 
    loadImage() 함수를 사용하여 이미지를 로드하고, image() 함수를 사용하여 이미지를 표시할 수 있다. 아래 코드에서 preload() 함수는 이미지를 로드하고, setup() 함수에서 image() 함     수를 사용하여 이미지를 표시한다. 
    
    지형은 TRIANGLE_STRIP을 사용하여 그리며, 각 지형의 높이는 노이즈 함수의 값을 이용하여 계산한다.
   

let cols, rows;

let scl = 20;

let w = 1400;

let h = 1000;


let flying = 0;


let terrain = [];


let img1, img2;

function preload() {

  img1 = loadImage("sky.jpg");
  
  img2 = loadImage("bird.png");
  
}


function setup() {

  
  createCanvas(600, 600, WEBGL);
  
  cols = w / scl;
  
  rows = h / scl;
  
  noStroke();


  for (var x = 0; x < cols; x++) {
  
    terrain[x] = [];
    
    for (var y = 0; y < rows; y++) {
    
      terrain[x][y] = 0; //specify a default value for now
      
    }
    
  }
  
}function draw() {

  background(255);
  
  flying -= 0.1;
  
  let yoff = flying;
  
  for (var y = 0; y < rows; y++) {
  
    var xoff = 0;
    
    for (var x = 0; x < cols; x++) {
    
      terrain[x][y] = map(noise(xoff, yoff), 0, 1, -50, 50);
      
      xoff += 0.2;
      
    }
    
    yoff += 0.2;
    
  }
  

  // 배경 이미지 그리기
  
  image(img1, -310, -410, width, height/2);
  
  //image(img2, /100, 100, width, height/);
  

  translate(0, 50);
  
  rotateX(PI / 3);
  
  fill(200, 200, 200, 150);
  
  translate(-w / 2, -h / 2);
  
  for (let y = 0; y < rows - 1; y++) {
  
    beginShape(TRIANGLE_STRIP);
    
    for (let x = 0; x < cols; x++) {
    
      let v = terrain[x][y];
      
      fill(v+34, v+139, v+34)
      
      vertex(x * scl, y * scl, terrain[x][y]);
      
      vertex(x * scl, (y + 1) * scl, terrain[x][y + 1]);
      
    }
    
    endShape();
    
  }
  
  push(); translate(w / 2, h / 2);
  
  translate(mouseX - width / 2, (mouseY - height / 2) * 6);
  
  //rotate(PI / 5);
  
  rotateX(mouseY/10);
  
  rotateY(mouseX/10);
  
  normalMaterial();
  
  texture(img2);
  
  sphere(100); pop();
  
}


3. 결론 : 본 코드에서 여러 함수들을 통해 하늘 배경사진을 삽입후 3D 지형의 색을 변경 하고 하늘 위의 시점에서 높은 하늘과 많은 산맥들을 보는 듯한 장면을 구현 하고 구에 사진을 삽입후 마우스에 따라 날아다는 듯한 새를 표현하여 3D 그래픽을 구현하였다.  첨부한 graphics.mp4 영상을 통해 코드와 실행 장면을 눈으로 확인 가능하다. 크게 컴퓨터 그래픽스를 활용한 분야를 두가지가 있다. 첫번째, 게임 개발에 활용 할수 있다. 게임은 컴퓨터 그래픽스 기술을 많이 활용하는 대표적인 분야다. 게임 개발자는 3D 모델링, 애니메이션, 물리 시뮬레이션 등의 기술을 사용하여 게임을 만들어낸다. 두번째, 영상제작에 활용 할수 있다. 영화나 TV 프로그램, 광고 등에서도 컴퓨터 그래픽스 기술이 많이 사용된다. VFX(Visual Effects) 기술을 사용하여 실제로 촬영한 영상에 다양한 특수 효과를 추가하거나, 3D 애니메이션을 만들어내는 등의 작업을 수행한다. 이런 기초 예제를 학습하면 향후 Editor가 아닌 컴퓨터의 여러 상황에서 볼수 있는 다양한 함수를 가진 그래픽스를 구현하게 될것이다.

4. 참고 : chat gpt의 자세한 대화, Coding Challenge 11: 3D Terrain Generation with Perlin Noise in Processing
