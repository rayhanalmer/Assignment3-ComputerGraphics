# Assignment3-ComputerGraphics

Name    : Rayhan Almer Kusumah  
NRP     : 5025211115  
Class   : Computer Graphics (A)  

## 2D-SQUARE

Drawing a 2-D square by combining two triangles.  
Preview: [2-D Square Preview](https://rayhanalmer.github.io/Assignment3-ComputerGraphics/2D-Square.html "square")  

The square was created by positioning two separate 
triangles at specific coordinates in a way that they 
combine to form a unified square shape. 

|![triangle1](https://github.com/rayhanalmer/Assignment3-ComputerGraphics/blob/main/assets/triangle-1.png)|![triangle2](https://github.com/rayhanalmer/Assignment3-ComputerGraphics/blob/main/assets/triangle-2.png)|![square](https://github.com/rayhanalmer/Assignment3-ComputerGraphics/blob/main/assets/square.png)|
|-------------------|-------------------|--------------------|

Below is a code snippet illustrating how this drawing and positioning were accomplished:  

``` javascript
function draw() { 
    
        gl.clearColor(0,0,0,1);  
        gl.clear(gl.COLOR_BUFFER_BIT);  
    
        /* FIRST TRIANGLE */
        let  coords1 = new Float32Array( [ -0.5,-0.5, 0.5,0.5, -0.5,0.5 ] );
       
        gl.bindBuffer(gl.ARRAY_BUFFER, bufferCoords);
        gl.bufferData(gl.ARRAY_BUFFER, coords1, gl.STREAM_DRAW);
        gl.vertexAttribPointer(attributeCoords, 2, gl.FLOAT, false, 0, 0);
        gl.enableVertexAttribArray(attributeCoords); 
       
        // Set up values for the "color" attribute 
       
        let  color1 = new Float32Array( [ 0,1,0, 1,0,0, 0,0,1 ] );
    
        gl.bindBuffer(gl.ARRAY_BUFFER, bufferColor);
        gl.bufferData(gl.ARRAY_BUFFER, color1, gl.STREAM_DRAW);
        gl.vertexAttribPointer(attributeColor, 3, gl.FLOAT, false, 0, 0);
        gl.enableVertexAttribArray(attributeColor); 
        
        // Draw the triangle.
       
        gl.drawArrays(gl.TRIANGLES, 0, 3);
        
         /* SECOND TRIANGLE */
         let  coords2 = new Float32Array( [ -0.5,-0.5, 0.5,0.5, 0.5, -0.5 ] );
       
         gl.bindBuffer(gl.ARRAY_BUFFER, bufferCoords);
         gl.bufferData(gl.ARRAY_BUFFER, coords2, gl.STREAM_DRAW);
         gl.vertexAttribPointer(attributeCoords, 2, gl.FLOAT, false, 0, 0);
         gl.enableVertexAttribArray(attributeCoords); 
        
         // Set up values for the "color" attribute 
        
         let  color2 = new Float32Array( [ 0,1,0, 1,0,0, 1,1,1 ] );
     
         gl.bindBuffer(gl.ARRAY_BUFFER, bufferColor);
         gl.bufferData(gl.ARRAY_BUFFER, color2, gl.STREAM_DRAW);
         gl.vertexAttribPointer(attributeColor, 3, gl.FLOAT, false, 0, 0);
         gl.enableVertexAttribArray(attributeColor); 
         
         // Draw the triangle.
        
         gl.drawArrays(gl.TRIANGLES, 0, 3);
    }
```

## 3D-CUBE

Drawing a 3-D rotation cube.  
Preview: [3-D Cube Preview](https://rayhanalmer.github.io/Assignment3-ComputerGraphics/3D-Cube.html "cube")  

The cube was made  by modifying the initial sample code and then animated to visualize each angle by dragging on your mouse. 


![cube](https://github.com/rayhanalmer/Assignment3-ComputerGraphics/blob/main/assets/cube-rotation.gif)

Here are snippets of the code that depict the drawing and arrangement process :

### Drawing

``` javascript
function draw() { 
    gl.clearColor(0,0,0,1);
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    
    /* Set the value of projection to represent the projection transformation */
    
    mat4.perspective(projection, Math.PI/8, 1, 8, 12);
    
    /* Get the view matrix from the SimpleRotator object.
       There is no modeling transformation in this program. */
    
    let modelview = rotator.getViewMatrix();
        
    /* Multiply the projection matrix times the modelview matrix to give the
       combined transformation matrix, and send that to the shader program. */
       
    mat4.multiply( modelviewProjection, projection, modelview );
    gl.uniformMatrix4fv(u_modelviewProjection, false, modelviewProjection );
    
    /* Draw the six faces of a cube, with different colors. */
    
    // Front (Red)
    drawPrimitive( gl.TRIANGLE_FAN, [1,0,0,1], [ -1,-1,1, 1,-1,1, 1,1,1, -1,1,1 ]);
    // Top (Green)
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,0,1], [ -1,-1,-1, -1,1,-1, 1,1,-1, 1,-1,-1 ]);
    // Right (Blue)
    drawPrimitive( gl.TRIANGLE_FAN, [0,0,1,1], [ -1,1,-1, -1,1,1, 1,1,1, 1,1,-1 ]);
    // Bottom (Yellow)
    drawPrimitive( gl.TRIANGLE_FAN, [1,1,0,1], [ -1,-1,-1, 1,-1,-1, 1,-1,1, -1,-1,1 ]);
    // Left (Magenta)
    drawPrimitive( gl.TRIANGLE_FAN, [1,0,1,1], [ 1,-1,-1, 1,1,-1, 1,1,1, 1,-1,1 ]);
    // Back (Cyan)
    drawPrimitive( gl.TRIANGLE_FAN, [0,1,1,1], [ -1,-1,-1, -1,-1,1, -1,1,1, -1,1,-1 ]);
    
    /* Draw coordinate axes as thick colored lines that extend through the cube. */
    
    gl.lineWidth(4);
    drawPrimitive( gl.LINES, [1,0,0,1], [ -2,0,0, 2,0,0] );
    drawPrimitive( gl.LINES, [0,1,0,1], [ 0,-2,0, 0,2,0] );
    drawPrimitive( gl.LINES, [0,0,1,1], [ 0,0,-2, 0,0,2] );
    gl.lineWidth(1);
}
```

### Javascript rotation by mouse function
``` javascript
function SimpleRotator(canvas, callback, viewDistance, rotY, rotX) {
    canvas.addEventListener("mousedown", doMouseDown, false);
    canvas.addEventListener("touchstart", doTouchStart, false);
    var rotateX = (rotX === undefined)? 0 : rotX;
    var rotateY = (rotY === undefined)? 0 : rotY;
    var xLimit = 85;
    var center;
    var degreesPerPixelX = 90/canvas.height;
    var degreesPerPixelY = 180/canvas.width; 
    this.getXLimit = function() {
        return xLimit;
    };
    this.setXLimit = function(limitInDegrees) {
        xLimit = Math.min(85,Math.max(0,limitInDegrees));
    };
    this.getRotationCenter = function() {
        return (center === undefined) ? [0,0,0] : center;
    };
    this.setRotationCenter = function(rotationCenter) {
        center = rotationCenter;
    };
    this.setAngles = function( rotY, rotX ) {
        rotateX = Math.max(-xLimit, Math.min(xLimit,rotX));
        rotateY = rotY;
        if (callback) {
            callback();
        }
    };
    this.getAngles = function() {
        return [rotateX,rotateY];
    };
    this.setViewDistance = function( dist ) {
        viewDistance = dist;
    };
    this.getViewDistance = function() {
        return (viewDistance === undefined)? 0 : viewDistance;
    };
    this.getViewMatrix = function() {
        var cosX = Math.cos(rotateX/180*Math.PI);
        var sinX = Math.sin(rotateX/180*Math.PI);
        var cosY = Math.cos(rotateY/180*Math.PI);
        var sinY = Math.sin(rotateY/180*Math.PI);
        var mat = [  // The product of rotation by rotateX about x-axis and by rotateY about y-axis.
            cosY, sinX*sinY, -cosX*sinY, 0,
            0, cosX, sinX, 0,
            sinY, -sinX*cosY, cosX*cosY, 0,
            0, 0, 0, 1
        ];
        if (center !== undefined) {  // multiply on left by translation by rotationCenter, on right by translation by -rotationCenter
            var t0 = center[0] - mat[0]*center[0] - mat[4]*center[1] - mat[8]*center[2];
            var t1 = center[1] - mat[1]*center[0] - mat[5]*center[1] - mat[9]*center[2];
            var t2 = center[2] - mat[2]*center[0] - mat[6]*center[1] - mat[10]*center[2];
            mat[12] = t0;
            mat[13] = t1;
            mat[14] = t2;
        }
        if (viewDistance !== undefined) {  // multipy on left by translation by (0,0,-viewDistance)
            mat[14] -= viewDistance;
        }
        return mat;
    };```


