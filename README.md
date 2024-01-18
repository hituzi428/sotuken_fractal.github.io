<html>
<script>
window.onload =function(){
 var $ = function(id){ return document.getElementById(id); }
 var canvas = $("canvas");
 var ctx = canvas.getContext('2d');
 var clear =function(){
  ctx.fillStyle = 'rgb(200, 200, 200)';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
 }
 var line = function(ctx, sx, sy, ex, ey){
  ctx.beginPath();
  ctx.moveTo(sx, sy);
  ctx.lineTo(ex, ey);
  ctx.closePath();
  ctx.stroke();
 }
 var dline = function(ctx, sx, sy, ex, ey, cx, cy){
  ctx.beginPath();
  ctx.moveTo(sx, sy);
  ctx.lineTo(cx, cy);
  ctx.moveTo(ex, ey);
  ctx.lineTo(cx, cy);
  ctx.closePath();
  ctx.stroke();
 };
 var dragon = function(ctx, depth, sx, sy, ex, ey, d){
  if(depth ==0){
   line(ctx,sx,sy,ex,ey);
   return;
  }
  var dx = (ex-sx)/2;
  var dy = (ey-sy)/2;
  var centerX = ex-dx;
  var centerY = ey-dy;
  var cx = centerX + (d?dy:-dy);
  var cy = centerY + (d?-dx:dx);
  if(depth == 1){
   dline(ctx,sx,sy,ex,ey,cx,cy);
   return;
  }
  else{
   dragon(ctx, depth-1, sx, sy, cx, cy, true);
   dragon(ctx, depth-1, cx, cy, ex, ey, false);
  }
 }
 var dragon_call = function(depth){
  return function(){
   clear();
   dragon(ctx, depth, 50, canvas.height/3, canvas.width-100, canvas.height/3,false);
   if(depth < 15){
    setTimeout(dragon_call(depth+1), 1000);
   }
  }
 };
 $("start").onclick = function(){
  dragon_call(0)();
 }
 dragon_call(15)();
}
</script>
<body>

<canvas id="canvas" width=400 height=400 ></canvas>
<br/>
<input type="button" id="start" value="start" ></input>
</body>
</html>
