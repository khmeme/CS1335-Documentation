.. _pshapeObjects:

===============
PShape Objects
===============

The Processing website has a good tutorial about `PShape`_ objects, so it's advisable that you
review that tutorial in order to have a basic understanding of `PShape`_ objects.  The idea is
that `PShape`_ allows us to create geometric primitives that we can use as objects.  There are 
a variety of ways to use PShape, let's start by creating a Star class. 

Stars
=======
In the Processing `PShape`_ tutorial, the authors create a Star class, and they use the createShape(), beginShape()
and endShape() functions, along with a list of vertices in the Star constructor to create the geometry for the
Star object.  

However, they go on to describe a subtle concept; the idea of creating the star geometry one time in
the main setup() function, and then passing that PShape object as an input parameter to the Star constructor.  This 
can cut down on rendering costs, where Processing can essentially 'memorize' the geometry of the single PShape object, 
rather than having to render the geometry for each one.  

For now, let's ignore these suggestions, and we'll create the geometry in the Star constructor function.  
Below are snippets of code for the Star class, where it inherits from the Drop class.  For our current 
version of the RainDrop game, the Drop class does not inherit from any other class::

	class Star extends Drop{  
		PShape s;
		
		Star(float _x){  // constructor lets us set the x-position
			super(_x);  // call the Drop constructor as the first line of code
		
		// First create the shape
			s = createShape();
			s.beginShape();
			// You can set fill and stroke
			s.fill(102);
			s.stroke(255);
			s.strokeWeight(2);
			// Here, we are hardcoding a series of vertices
			s.vertex(0, -50);
			s.vertex(14, -20);
			s.vertex(47, -15);
			s.vertex(23, 7);
			s.vertex(29, 40);
			s.vertex(0, 25);
			s.vertex(-29, 40);
			s.vertex(-23, 7);
			s.vertex(-47, -15);
			s.vertex(-14, -20);
			s.endShape(CLOSE);
			}
		}
	
The code above is taken directly from the Processing `PShape`_ Tutorial, except that we've made the
Star class a child class of the Drop class. 

Because the Star class inherits from the Drop class, we don't need to explicitly
declare most of the instance variables.  However, notice that in the first line of 
code in the constructor that we're calling ``super()``, this is the Drop class default constructor. 

Shiffman notes in the `PShape`_ Tutorial that by default, a PShape geometry is defined relative to the canvas origin (0,0). 
Therefore, he notes that it's we'll almost always use ``pushMatrix()``, ``popMatrix()``, and ``translate()`` in order to locate
our PShape objects on the canvas.

So, to write a ``display()`` method for our Star, we need to remember to translate the origin of the canvas
to the x,y coordinates of our shape before displaying using the `shape()` function.  Here's a display method that
also lets us set the color of the PShape object::

	 void display(){
		pushMatrix();
		translate(x,y);    //x,y were given default settings
  		s.setFill(color(0,200,127));   //we can change the fill color here if we want
		shape(s,0,0);
		popMatrix();
  }
  
If we want to determine where the center of the PShape is, what code can we write?
The easiest thing to do is to note that we've translated the origin to the x,y position, so
if we create an ellipse at (0,0) then we'll be able to observe where the center is. 

PShape using SVG Image File
============================

Another way that we can use PShape is to load an .svg file.  There are may sources for .svg files, for
this project, I'm using an svg file from `The Noun Project`.  This site has lots of .svg icon files
that are free for use if you provide proper attribution for the designer of the .svg file.  I'm using
an .svg file of a seahorse designed by: Agne Alesiute at http://thenounproject.com/ .  I am also using
an .svg file of a jellyfish designed by: Eli Ratner.  Below is the jellyfish .svg file.  We can see that 
the image has a text component where the designer's name is listed.  While we need to provide credit to 
the designers in our programs, we need to remove that text in order for this .svg file to work in our
programs. 

.. figure:: /images/jellyfish.png

Edit SVG Files
===============

There are 2 ways that we can edit .svg files.  Since .svg files are a standardized data format file
that list vertices for geometry and font elements, we can open the files in either a vector software
system like Adobe Illustrator, or Inkscape.  We can also open the files in any text editor like notePad
or textEdit. If we open the file in a text editor, then we need to go to the bottom of the file and 
find the ``<text> </text>`` content section.  We need to carefully select this content from the opening tag
to the closing tag, and the delete that content, being careful not to delete anything else.  In addition,
if we look at the .svg file, we can determine the width and height of the object, in addition to other 
image details.  Below is the .svg code for the image above::

	<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Layer_1" x="0px" y="0px" width="100px" height="100px" viewBox="5.0 -10.0 100.0 135.0" enable-background="new 0 0 100 100" xml:space="preserve">
	<g id="Layer_1_1_">
	<g id="Your_Icon">
	</g>
	</g>
	<path fill="#000000" d="M50,8.806c-15.769,0-28.552,10.665-28.552,23.821c0,3.51,1.188,7.938,2.544,9.844  c0.215,0.48,1.37,0.916,3.2,1.295c-0.025,0.049-0.054,0.096-0.078,0.146l-0.67,1.557c-0.452,1.051-0.883,2.141-1.193,3.301  c-0.314,1.153-0.508,2.403-0.34,3.685c0.023,0.174,0.042,0.287,0.088,0.51c0.037,0.138,0.062,0.258,0.134,0.459  c0.124,0.367,0.275,0.639,0.445,0.92c0.342,0.547,0.73,0.984,1.053,1.406c0.324,0.414,0.571,0.83,0.656,1.232  c0.099,0.525,0.104,1.002,0.039,1.539c-0.128,1.053-0.515,2.117-0.831,3.217c-0.311,1.098-0.532,2.283-0.34,3.445  c0.022,0.146,0.055,0.299,0.093,0.43c0.048,0.152,0.104,0.293,0.171,0.426c0.132,0.268,0.303,0.5,0.442,0.738  c0.297,0.465,0.347,1.051-0.025,1.469c-0.112,0.127-0.169,0.305-0.139,0.484c0.054,0.316,0.354,0.531,0.672,0.479  c0.317-0.055,0.531-0.355,0.478-0.674l-0.023-0.135c-0.244-1.443-1.247-2.176-1.298-3.266c-0.121-1.09,0.156-2.172,0.551-3.205  c0.381-1.045,0.893-2.068,1.169-3.242c0.139-0.572,0.197-1.236,0.146-1.83c-0.069-0.727-0.384-1.326-0.698-1.836  c-0.317-0.506-0.635-0.953-0.848-1.393c-0.103-0.209-0.198-0.451-0.233-0.625c-0.023-0.066-0.046-0.225-0.068-0.361l-0.02-0.33  c-0.031-0.975,0.224-1.984,0.56-2.982c0.351-0.999,0.807-1.99,1.28-2.996l0.733-1.523c0.092-0.209,0.162-0.437,0.208-0.675  c0.696,0.104,1.443,0.204,2.242,0.296c-0.135,0.273-0.256,0.554-0.383,0.822c-0.45,1.025-0.804,2.096-1.046,3.194  c-0.495,2.193-0.506,4.501-0.041,6.692c0.454,2.189,1.385,4.176,2.161,6.059c0.795,1.883,1.358,3.75,1.363,5.713  c0.034,1.969-0.49,3.992-0.505,6.205c-0.005,1.096,0.157,2.219,0.52,3.258c0.386,1.074,0.951,1.979,1.467,2.873  c0.517,0.891,0.98,1.777,1.179,2.736c0.211,0.977,0.242,1.994,0.178,3.012c-0.066,1.021-0.227,2.043-0.399,3.068  c-0.17,1.029-0.387,2.043-0.772,3.066c-0.028,0.076-0.042,0.164-0.031,0.25c0.035,0.281,0.292,0.482,0.575,0.447  s0.483-0.293,0.448-0.576c-0.129-1.043,0.063-2.074,0.283-3.092c0.218-1.02,0.427-2.051,0.543-3.107  c0.113-1.055,0.13-2.141-0.052-3.213c-0.179-1.102-0.658-2.109-1.143-3.035c-0.481-0.93-0.994-1.83-1.251-2.744  c-0.267-0.947-0.348-1.924-0.286-2.908c0.117-1.961,0.772-3.998,0.879-6.209c0.126-2.225-0.436-4.461-1.157-6.443  c-0.718-1.998-1.429-3.875-1.772-5.787c-0.336-1.898-0.242-3.848,0.202-5.713c0.224-0.933,0.544-1.844,0.948-2.719  c0.23-0.547,0.558-1.003,0.732-1.58c1.026,0.092,2.106,0.175,3.227,0.25c-0.262,0.611-0.484,1.242-0.658,1.895  c-0.441,1.578-0.768,3.236-0.766,4.949c-0.037,1.738,0.485,3.504,1.275,4.961c0.774,1.482,1.72,2.768,2.525,4.086  c1.672,2.559,2.362,5.645,2.853,8.748l0.67,4.695c0.121,0.785,0.253,1.568,0.413,2.348c0.162,0.781,0.3,1.555,0.221,2.41  c-0.011,0.123,0.02,0.258,0.098,0.365c0.172,0.24,0.506,0.295,0.746,0.123c0.24-0.172,0.295-0.506,0.124-0.746  c-0.483-0.674-0.673-1.447-0.79-2.223c-0.12-0.777-0.212-1.559-0.293-2.344l-0.437-4.725c-0.169-1.58-0.344-3.174-0.698-4.752  c-0.335-1.578-0.896-3.172-1.678-4.598c-0.753-1.434-1.596-2.768-2.195-4.139c-0.598-1.377-0.956-2.746-0.879-4.183  c0.05-1.455,0.385-2.921,0.847-4.369c0.259-0.776,0.526-1.56,0.729-2.378c1.299,0.07,2.64,0.127,4.007,0.173  c-0.416,1.857-0.692,3.988-0.327,6.071c0.182,1.062,0.509,2.08,0.909,3.037c0.42,0.977,0.835,1.805,1.305,2.68  c0.921,1.719,1.863,3.326,2.479,5.023c0.649,1.707,0.882,3.479,1.122,5.43c0.256,1.912,0.568,3.854,1.127,5.742  c0.548,1.893,1.389,3.686,2.41,5.334c0.499,0.83,1.057,1.604,1.355,2.473c0.313,0.863,0.312,1.848-0.089,2.738  c-0.045,0.1-0.06,0.215-0.034,0.332c0.063,0.283,0.346,0.463,0.631,0.4c0.285-0.064,0.465-0.346,0.401-0.631l-0.002-0.008  c-0.226-1.012-0.211-1.988-0.473-2.967c-0.27-0.977-0.799-1.814-1.231-2.656c-0.892-1.678-1.578-3.441-1.98-5.27  c-0.406-1.832-0.515-3.719-0.607-5.625c-0.051-0.951-0.054-1.906-0.158-2.922c-0.098-1-0.29-2.002-0.563-2.98  c-0.547-1.969-1.427-3.762-2.183-5.49c-0.364-0.852-0.783-1.754-1.073-2.561c-0.299-0.828-0.513-1.664-0.622-2.499  c-0.256-1.67,0.034-3.352,0.443-5.187c0.025-0.127,0.048-0.258,0.072-0.388c2.275,0.045,4.588,0.056,6.87,0.028  c-0.04,0.302-0.072,0.607-0.093,0.919c-0.108,1.711-0.159,3.514,0.156,5.335c0.316,1.849,1.002,3.542,1.729,5.062  c0.713,1.535,1.454,2.959,1.907,4.418c1.034,2.854,0.114,6.096-0.254,9.496c-0.166,1.703-0.111,3.498,0.511,5.166  c0.3,0.826,0.822,1.545,1.228,2.242c0.411,0.699,0.682,1.465,0.567,2.211c-0.009,0.055-0.009,0.115,0.003,0.174  c0.054,0.277,0.321,0.457,0.598,0.404c0.276-0.055,0.457-0.322,0.403-0.6l-0.006-0.027c-0.367-1.887-1.606-3.17-2.023-4.684  c-0.444-1.506-0.374-3.137-0.056-4.713c0.302-1.59,0.822-3.168,1.17-4.865c0.173-0.834,0.307-1.748,0.334-2.639  c0.025-0.92-0.065-1.842-0.248-2.725c-0.356-1.777-1.015-3.398-1.588-4.928c-0.567-1.539-1.067-2.991-1.229-4.448  c-0.176-1.478-0.141-3.026,0.012-4.629c0.036-0.402,0.066-0.814,0.078-1.235c1.143-0.033,2.269-0.076,3.368-0.13  c0.006,0.46,0.045,0.915,0.108,1.377L60.32,49.9c0.2,2.252,0.441,4.522,0.707,6.797c0.142,1.139,0.301,2.275,0.497,3.41  c0.211,1.141,0.434,2.236,0.571,3.355c0.146,1.119,0.238,2.25,0.3,3.381c0.037,1.131,0.194,2.273-0.289,3.395  c-0.053,0.123-0.061,0.268-0.01,0.402c0.104,0.279,0.415,0.42,0.694,0.316c0.278-0.105,0.42-0.416,0.315-0.693  c-0.433-1.154-0.268-2.285-0.271-3.43c0.004-1.141-0.008-2.283-0.047-3.43c-0.049-1.15-0.146-2.307-0.227-3.42  c-0.069-1.125-0.098-2.254-0.118-3.385c-0.046-2.264,0.004-4.539-0.073-6.826c-0.023-1.145-0.103-2.291-0.178-3.441  c-0.023-0.429-0.079-0.863-0.163-1.293c1.365-0.088,2.676-0.195,3.914-0.32c-0.368,0.834-0.695,1.669-0.978,2.616  c-0.164,0.583-0.285,1.224-0.278,1.926c0,0.707,0.223,1.421,0.49,1.968c0.534,1.082,1.1,1.969,1.51,2.924  c0.409,0.953,0.674,1.99,0.839,3.059c0.149,1.07,0.212,2.168,0.215,3.27c-0.006,1.104,0.094,2.207-0.367,3.311  c-0.051,0.121-0.058,0.266-0.007,0.4c0.105,0.277,0.416,0.418,0.694,0.312s0.418-0.418,0.312-0.695  c-0.42-1.105-0.197-2.207-0.127-3.311c0.077-1.105,0.168-2.221,0.15-3.354c-0.001-1.133-0.109-2.289-0.404-3.432  c-0.302-1.141-0.793-2.203-1.104-3.162c-0.147-0.475-0.193-0.855-0.167-1.255c0.03-0.404,0.158-0.852,0.32-1.321  c0.319-0.93,0.811-1.945,1.224-2.918c0.097-0.216,0.194-0.432,0.291-0.648c1.137-0.157,2.181-0.334,3.113-0.534  c-0.216,0.735-0.35,1.503-0.392,2.288c-0.107,1.377,0.165,2.771,0.621,4.012c0.437,1.253,0.993,2.424,1.463,3.631  c0.231,0.604,0.486,1.203,0.67,1.807c0.193,0.592,0.277,1.275-0.183,1.822c-0.138,0.162-0.183,0.395-0.098,0.605  c0.122,0.307,0.47,0.455,0.775,0.332c0.306-0.121,0.455-0.469,0.332-0.775l-0.018-0.043c-0.297-0.742-0.341-1.369-0.487-2.027  c-0.157-0.646-0.378-1.254-0.574-1.871c-0.399-1.233-0.785-2.479-1.074-3.713c-0.282-1.239-0.378-2.457-0.21-3.664  c0.135-0.914,0.391-1.809,0.59-2.748c1.753-0.486,2.928-1.075,3.296-1.781c1.112-2.063,2.168-5.891,2.168-9.122  C78.552,19.471,65.77,8.806,50,8.806z"/>
	<text x="0.0" y="117.5" font-size="5.0" font-weight="bold" font-family="Helvetica Neue, Helvetica, Arial-Unicode, Arial, Sans-serif" fill="#000000">Created by Eli Ratner</text><text x="0.0" y="122.5" font-size="5.0" font-weight="bold" font-family="Helvetica Neue, Helvetica, Arial-Unicode, Arial, Sans-serif" fill="#000000">from the Noun Project</text>
	</svg>

So, delete the <text> </text> line in the code above, however, make sure not to delete the closing </svg> tag. Also, 
in the first line of code, if the view box is significantly different than the width and height, which are usually
100px by 100px, then this can cause problems. You will probably want to pick a different image. 

In order to use an svg file for a PShape object, it's necessary to use the following syntax in 
order to load the image.  

	1.  The .svg file must be put inside a folder named: ``data``, inside your sketch folder
	2.  PShape s= loadShape("seaHorse2.svg");  // this loads the image 
	3.  shape(s, x, y, width, height) ;  //this is used to display the svg.
	
SVG Origin
===========

With SVG files, the x,y position refers to the top left corner of the svg file.  If you open the
svg file in a text editor, you can read the width and height dimensions of the svg.  Those can help us
when we try to determine the bounding box for collision detection.  Below is the display function 
that is overriding the default style of the svg to allow us to reset the fill color.  Below that I've
added a rectangle to show the bounding box.  Since we're using translate(x,y) so that we've moved the 
canvas origin to the svg corner point, then we'll draw the rectangle at (0,0).::

	void display(){
		pushMatrix();
		translate(x,y);
		s.disableStyle();  // Ignore the colors in the SVG
		fill(c);    // Set the SVG fill to myColor
		stroke(55);          // Set the SVG fill to gray
		shape(s,0,0,sWidth,sHeight);
		noFill();
		rect(0,0,sWidth,sHeight);  //bounding box 
		popMatrix();
		}
		

Bounding Box
=============

In order to determine if we have a collision with the paddle, we need to define a bounding box
for our PShape object that we'll use to determine contact with the Paddle object.  Shiffman's
game used circular objects, and determining intersection with circular objects is a bit easier than
it is with rectangular objects.  As noted above, we've translated the origin to the x,y position
of our .svg file, this corresponds to the upper-upper left corner of the shape.  

.. _PShape:  https://processing.org/tutorials/pshape/