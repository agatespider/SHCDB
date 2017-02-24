# (미정리)

# 1.개요
한번 보았던 책을 다시 깊이 있게 정리를 하는게 주 목적

# 2. 책을 읽기전에 알아두어야 할 목록입니다.
* Normal Flow
* Box Model
* BFC와 IFC
* Block과 Inline
* Display와 Float

윽 예상외로 많습니다. 그리고 처음보는 용어도 많습니다. 이것들이 다 무엇일까요?

## 2.1 Normal Flow
html의 요소들의 위치를 정하는 방식은 여러가지가 있습니다. 그중 기본값은 static position이라고 하는데요. 여기서의 static은 정적이다라는 뜻이 아닌 컴퓨터가 알아서 그림을 그려준다는 의미를 가지고 있습니다.

그렇다면 컴퓨터가 어떤방식으로 Element를 알아서 그려주는것인가요?

html을 그릴때 static position 형태로 그리는 방법을 Normal Flow라고 합니다.

이 Normal Flow는 Box Model로 Element의 크기를 계산하구요. 2가지의 배치 방식(BFC, IFC)으로 요소의 위치를 정해 그려줍니다.

## 2.2 Box Model
Box Model이란 HTML에 하나의 Element를 사각형 박스로 영역을 정하고 그리는 모델을 뜻합니다. 가장 쉽게 볼수 있는 방법은 크롬에서 F12를 눌러서 개발자 도구를 연후 우측 최하단에 표시가 됩니다.
 
![boxModel](https://github.com/agatespider/SHCDB/blob/master/chapter01/docimg/boxmodel.PNG)

각각의 영역은 다음과 같습니다.
* margin : 바깥을 감싸고 있는 부분으로 다른 Element와 구별을 하기 위해 쓰의는 빈 영역
* border : Element의 외각선 영역
* padding : border를 시작으로 contents까지의 영역
* content : Element의 실제 영역

위 4개의 영역을 바탕으로 Geometry 공간을 계산하고 배치를 하게 됩니다. 그리고 이 요소는 width와 height 그리고 box-sizing속성과 밀접한 연관이 있습니다.

[box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)은 Element의 width와 height를 계산하는 방식을 결정하는 속성입니다.

이 속성에는 context-box, padding-box, border-box라는 3가지의 값이 있습니다. 이름을 봐도 알 수 있듯이 각각의 기능은 다음과 같습니다.

* content-box: width와 height를 content영역만을 사용합니다.
* padding-box: content와 padding 영역을 사용합니다.
* border-box: content와 padding, border 영역을 사용합니다.

즉 width와 height에 맞추어서 box-sizing속성에 따라 다이나믹하게 계산을 해주는 겁니다.

box-sizing이 border-box이고 width가 200, height 200이고 boder가 10, 패딩이 10일경우 실제 컨텐츠의 사이즈는 200-10-10 = 180으로 잡는다는 것입니다.

왜 box-sizing을 사용하나면 예를 들면 부모 자식간의 관계에서 자식의 width를 100%으로 주고 자식과 부모와의 여백이 조금 있어야 할 경우 사용 할 수 있는데, 자식에 margin이나 border를 주어버리면 100% + margin값이 계산되어 부모의 영역을 벗어나 버립니다.

이럴경우 content사이즈를 부모값에 마추는게 중요한데요, margin을 0으로 하고 box-sizing을 border-box로 하면 자식의 content영역은 padding과 border를 계산하고 남은 값을 설정하기 때문에 생각한데로 만들 수 있기 때문입니다.

## 2.3 Normal Flow의 2가지 배치 방식 BFC, IFC
Normal Flow는 Box Model로 크기를 계산하고 2가지의 배치 방식으로 Element를 그린다고 했었습니다.

이 2가지의 배치방식은 BFC(Block Formatting Context)와 IFC(Inline Formatting Context)입니다.

말그대로 BFC는 Element를 세로로 배치하는 영역를 뜻하며 IFC는 Element를 가로로 배치하는 영역을 뜻합니다.

즉 BFC는 BFC영역안의 요소들을 세로 배치 할 수 있도록 위치를 자동으로 계산을 해주고 IFC는 IFC영역안의 요소들을 가로 배치 할 수 있도록 위치를 자동으로 계산을 해주는 것입니다.

BFC를 만드는 조건은 아래와 같습니다.
* Root 또는 이를 포함하는 요소(Body)
* float가 none이 아닌 Element
* 절대 위치로 지정된 Element (Position이 absolute 또는 fixed)
* display가 inline-block, table-cell, table-caption인 Element
* overflow가 visible이 아닌 Element
* flex box (display가 flex 또는 inline-flex)인 Element

그리고 BFC의 특징 중 하나가 있는데 자식 BFC의 자식 항목을 제외한 모든것을 포함해주는 기능이 있습니다. 

간단히 설명하면 부모 BFC는 자식 BFC안의 손자를 제외한 나머지 항목을 포함해서 표시 해준다고 생각하면 됩니다. 또한 자식의 영역을 잡을때 한 행을 block처럼 해서 잡습니다. 그리고 IFC는 이 BFC영역에 들어올수 없습니다.

그리고 BFC는 자신을 어떻게 배치를 해주느냐 하는것도 정해줍니다.

스팩문서를 보면 BFC는 아래와 같은 역할을 합니다.
 
BFC의 배치는 각 Element의 좌상단 가장자리가 블록의 왼쪽 가장자리에 닿습니다 (오른쪽에서 왼쪽으로 계산, 오른쪽 가장자리에서 시작).

BFC각각의 BFC Element를 배치할때 좌상단 꼭지점을 무조건 좌측에 붙이려고 합니다. 이것에 대한 계산은 오른쪽에서 왼쪽으로 진행하면서 계산을 합니다. 그런데 만약 BFC엘리먼트가 먼저 그려저 있다면 우측에서 계산을 시작합니다.

그런데 이전에 있는 BFC가 float Element일 경우 float기능으로 인해 나의 Element와 이전 Element의 width가 부모의 width값을 초과하지 않는다면 이전 BFC 오른쪽 위 가장자리에서 랜더링을 하자라는 뜻입니다. 이것은 차후 예제 컬럼 탈락에서도 왜 이렇게 되는지 알수 있게 해줍니다.

     <style>
            .boxA:after {
                content: "";
                display: block;
                clear: both;
            }
    
            .box2 {float: left; width: 50%; }
            .box3 {float: left; width: 25%; height:50px; line-height: 50px;}
            .box1 {float: left; width: 25%; height:100px; line-height: 100px;}
            .box4 {
                overflow: hidden; width: 10%;
            }
        </style>
    </head>
    <body>
        <div class="boxA">
            <div class="box1">
                BOX1
            </div>
            <div class="box2">
                BOX2
            </div>
            <div class="box3">
                BOX3
            </div>
            <div class="box4">
                BOX4
            </div>
        </div>
    </body>

 




