# computer_graphicks
<input type=text"
ДЗ 1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cube</title>
    <style>
        body {
            margin: 0;
        }
        canvas {
            display: block;
        }
    </style>
</head>
<body>
<script>
    //consts 
    const COLOUR_bg   = "black";
    const COLOUR_cube = "white";
    const COLOUR_roof = "green";
    const COLOUR_door = "brown";
    const COLOUR_wi   = "blue";
    const SPEED_x = 0.05;
    const SPEED_y = 0.10;
    const SPEED_z = 0.15;
    const POINT3D = function(x,y,z){this.x=x; this.y=y; this.z =z;}
      
    //contex and canvas 
    var canvas =document.createElement("canvas");
    document.body.appendChild(canvas) ;
    var ctx  = canvas.getContext("2d");
    //var ctx2 = canvas.getContext("2d");
    
    //dimensions
    var w = document.documentElement.clientWidth;
    var h = document.documentElement.clientHeight;
    canvas.height = h;
    canvas.width = w;

    //colours and lines
    ctx.fillStyle = COLOUR_bg;
    ctx.lineWidth= w/100;
    ctx.lineCap="round";
  
    //cube parameters
    var cx=w/2;
    var cy=h/2;
    var cz=0;
  
  if ((h/6) > (w/6)){
    size = w/6;}
  else{
    size = h/6;}
 
   var verticies=[
     new POINT3D(cx-size, cy+size, cz-size),//0
     new POINT3D(cx+size, cy+size, cz-size),
     new POINT3D(cx+size, cy-size, cz-size),
     new POINT3D(cx-size, cy-size, cz-size),
     new POINT3D(cx-size, cy+size, cz+size),
     new POINT3D(cx+size, cy+size, cz+size),
     new POINT3D(cx+size, cy-size, cz+size),
     new POINT3D(cx-size, cy-size, cz+size),//7
      
     new POINT3D(cx,cy+(2*size),cz),//8

     new POINT3D(cx-size/2, cy+size/2, cz+size),
     new POINT3D(cx+size/2, cy+size/2, cz+size),
     new POINT3D(cx+size/2, cy-size, cz+size),
     new POINT3D(cx-size/2, cy-size, cz+size),//12
      
     new POINT3D(cx-size/2, cy+size/2, cz-size),
     new POINT3D(cx+size/2, cy+size/2, cz-size),
     new POINT3D(cx+size/2, cy-size/2, cz-size),
     new POINT3D(cx-size/2, cy-size/2, cz-size)//16
    ] 
    
    var edges = [
       [0, 1], [1, 2], [2, 3], [3, 0],//back face
       [4, 5], [5, 6], [6, 7], [7, 4],//front face
       [0, 4], [1, 5], [2, 6], [3, 7]// connecting 
     ];  
     var edges2 =[
       [8, 4], [8, 1], [8, 0], [8, 5],//roof
       [0, 5], [4, 1]
    ];
    var edges3=[
       [9,10],[10,11],[11,12],[12,9]
    ];
    var edges4=[
       [13,14],[14,15],[15,16],[16,13]
    ];
   
    // set up the animation loop
    var timeDelta, timeLast = 0;
    requestAnimationFrame(loop1);
   // requestAnimationFrame(loop2);
  

    function loop1(timeNow) {

        // calculate the time difference
        timeDelta = timeNow - timeLast;
        timeLast = timeNow;

        // background
        ctx.fillRect(0, 0, w, h);

        // rotate the cube along the z axis
        let angle = timeDelta * 0.001 * SPEED_z * Math.PI * 2;
        for (let v of verticies) {
            let dx = v.x - cx;
            let dy = v.y - cy;
            let x = dx * Math.cos(angle) - dy * Math.sin(angle);
            let y = dx * Math.sin(angle) + dy * Math.cos(angle);
            v.x = x + cx;
            v.y = y + cy;
        }

        // rotate the cube along the x axis
        angle = timeDelta * 0.001 * SPEED_x * Math.PI * 2;
        for (let v of verticies) {
            let dy = v.y - cy;
            let dz = v.z - cz;
            let y = dy * Math.cos(angle) - dz * Math.sin(angle);
            let z = dy * Math.sin(angle) + dz * Math.cos(angle);
            v.y = y + cy;
            v.z = z + cz;
        }

        // rotate the cube along the y axis
        angle = timeDelta * 0.001 * SPEED_y * Math.PI * 2;
        for (let v of verticies) {
            let dx = v.x - cx;
            let dz = v.z - cz;
            let x = dz * Math.sin(angle) + dx * Math.cos(angle);
            let z = dz * Math.cos(angle) - dx * Math.sin(angle);
            v.x = x + cx;
            v.z = z + cz;
        }

        // draw each edge
      //cube
        ctx.beginPath();
        ctx.strokeStyle = COLOUR_cube;
        for (let edge1 of edges) {
            ctx.moveTo(verticies[edge1[0]].x, verticies[edge1[0]].y);
            ctx.lineTo(verticies[edge1[1]].x, verticies[edge1[1]].y);
            ctx.stroke();
        }
        ctx.closePath();
      
     //roof
     ctx.beginPath();
     ctx.strokeStyle = COLOUR_roof;
     for (let edge2 of edges2){
            ctx.moveTo(verticies[edge2[0]].x, verticies[edge2[0]].y);
            ctx.lineTo(verticies[edge2[1]].x, verticies[edge2[1]].y);
            ctx.stroke();
        }
      ctx.closePath();         
     
     //window
     ctx.beginPath();
     ctx.strokeStyle = COLOUR_wi;
     for (let edge4 of edges4){
            ctx.moveTo(verticies[edge4[0]].x, verticies[edge4[0]].y);
            ctx.lineTo(verticies[edge4[1]].x, verticies[edge4[1]].y);
            ctx.stroke();
        }
      ctx.closePath();         
       
     //door
     ctx.beginPath();
     ctx.strokeStyle = COLOUR_door;
     for (let edge3 of edges3){
            ctx.moveTo(verticies[edge3[0]].x, verticies[edge3[0]].y);
            ctx.lineTo(verticies[edge3[1]].x, verticies[edge3[1]].y);
            ctx.stroke();
        }
      ctx.closePath();         
         
        // call the next frame
        requestAnimationFrame(loop1);
    }

    </script>
  </body>
</html>

Дз2
