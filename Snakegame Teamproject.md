# 스네이크게임 팀프로젝트
## 소스코드
``` java script
var s;
var scl = 20;
var food;
playfield = 600;
var a; //r 값에 들어갈 랜덤한 수를 저장할 변수
var b; //g 값에 들어갈 랜덤한 수를 저장할 변수
var c; //b 값에 들어갈 랜덤한 수를 저장할 변수
var img; //이미지를 저장할 변수
var p=5; //점수를 나눈 값을 저장할 변수

function preload(){
  img=loadImage("snake.png"); //preload 함수를 사용하여 이미지를 로드
}

function setup() {
  createCanvas(playfield, 640);
  background(51);
  s = new Snake();
  frameRate(p)
  pickLocation();
}

function draw() {
  background(51);
  scoreboard();
  if (s.eat(food)) {
    pickLocation();
  }
  s.death();
  s.update();
  s.show();

  fill (a,b,c); //숫자 대신 변수 입력
  ellipse(food.x+10, food.y+10, scl, scl);
}

function pickLocation() {
  var cols = floor(playfield/scl);
  var rows = floor(playfield/scl);
  food = createVector(floor(random(cols)), floor(random(rows)));
  food.mult(scl);

  for (var i = 0; i < s.tail.length; i++) {
    var pos = s.tail[i];
    var d = dist(food.x, food.y, pos.x, pos.y);
    if (d < 1) {
      pickLocation();
    }
  }
}

function scoreboard() {
  fill(0);
  rect(0, 600, 600, 40);
  fill(255);
  textFont("Georgia");
  textSize(18);
  text("Score: ", 10, 625);
  text("Highscore: ", 450, 625)
  text(s.score, 70, 625);
  text(s.highscore, 540, 625)
}

function keyPressed() {
  if (keyCode === UP_ARROW){
      s.dir(0, -1);
  }else if (keyCode === DOWN_ARROW) {
      s.dir(0, 1);
  }else if (keyCode === RIGHT_ARROW) {
      s.dir (1, 0);
  }else if (keyCode === LEFT_ARROW) {
      s.dir (-1, 0);
  }
}

function Snake() {
  this.x =0;
  this.y =0;
  this.xspeed = 1;
  this.yspeed = 0;
  this.total = 0;
  this.tail = [];
  this.score = 1;
  this.highscore = 1;

  this.dir = function(x,y) {
    this.xspeed = x;
    this.yspeed = y;
  }

  this.eat = function(pos) {
    var d = dist(this.x, this.y, pos.x, pos.y);
    if (d < 1) {
      this.total++;
      this.score++;
      a=random(255); //음식의 r값을 랜덤하게 지정
      b=random(255); //음식의 g값을 랜덤하게 지정
      c=random(255); //음식의 b값을 랜덤하게 지정
      p=(this.score/5)+5;//점수가 5증가할때마다 속도가 1씩 증가하도록 설정.
      frameRate (p);
      text(this.score, 70, 625);
      if (this.score > this.highscore) {
        this.highscore = this.score;
      }
      text(this.highscore, 540, 625);
      return true;
    } else {
      return false;
    }
  }

  this.death = function() {
    for (var i = 0; i < this.tail.length; i++) {
      var pos = this.tail[i];
      var d = dist(this.x, this.y, pos.x, pos.y);
      if (d < 1) {
        this.total = 0;
        this.score = 0;
        this.tail = [];
      }
    }
  }

  this.update = function(){
    if (this.total === this.tail.length) {
      for (var i = 0; i < this.tail.length-1; i++) {
        this.tail[i] = this.tail[i+1];
    }

    }
    this.tail[this.total-1] = createVector(this.x, this.y);

    this.x = this.x + this.xspeed*scl;
    this.y = this.y + this.yspeed*scl;

    this.x = constrain(this.x, 0, playfield-scl);
    this.y = constrain(this.y, 0, playfield-scl);


  }
  this.show = function(){
    fill(c,b,a);
    for (var i = 0; i < this.tail.length; i++) {
        ellipse(this.tail[i].x+10, this.tail[i].y+10, scl, scl);
    }
    image(img,this.x, this.y, scl, scl);//rect 대신 이미지를 생성
  }
}
```
## 실행결과
![1](img/Snakegame.mp4)
## 변형내용
* 뱀의 먹이 색 랜덤으로 변경
* 뱀의 움직임 속도가 고정되지 않고 점수가 5점씩 증가할 때마다 속도 증가
* 뱀의 머리 이미지 첨부
* 뱀의 몸통 직사각형에서 원으로 변경
