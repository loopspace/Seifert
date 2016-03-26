# Seifert Surfaces in TikZ

This is a prototype LaTeX+TikZ document for drawing cutout Seifert
surfaces.
The goal is to make it easy to produce a figure that can be cut out
and stuck together to form a Seifert surface of a knot or link, which
can then itself be cut to find the boundary (and thus the knot).

In its current form, it is a load of code that needs to be stuck in a
document's preamble but once it stabilises it may well find its way
into a TikZ library.

## Instructions

Given the intended purpose of this code, I consider it highly unlikely
that it will be used as part of a larger document but rather as a
standalone for producing diagrams for a given project.

Thus the simplest way to get started is to copy the `Seifert.tex` file
and modify it directly.

Here's a simple example of a surface:

~~~
\begin{tikzpicture}[remember picture, overlay]
\coordinate (o) at (current page.center);
\pic[
  seifert/width=1cm,
  seifert/length=2cm,
  seifert/radius=3cm,
] at (o) {surface={
    {(4,0),90,-1,A},
    {(0,0),90,0,B},
    {(-4,0),90,1,C}%
}};
\end{tikzpicture}
~~~

The `remember picture, overlay` options are to enable us to draw the
surface without worrying about overful hboxes or vboxes.
We want to fill the page.
The `\coordinate` line provides an alias for the centre of the page
(note that you'll need two compilation runs before the picture
appears).
The magic goes on in the `\pic` command.

A piece of the surface is specified by giving a list of coordinates going
anti-clockwise around it.
At a coordinate, it is possible to add a segment where this piece
joins to another piece.   
These joins consist of strips that curve round.
Once cut out, these curves should be curled to point downwards.
They can then be glued to the corresponding join on another piece.
The joins are specified by giving an initial direction (in the form of an
angle) and a curve direction, one of -1, 0, or 1.
The join can also be labelled to make it easier to match with other
pieces.

Not every coordinate on the shape has to be a join.
If the join information is not specified then the coordinate is used just
to shape the piece.
The boundary is forced to go between these points.

There are also several configuration details which change various
dimensions of the shapes.
These can be seen in the above.

## Extra Details

1. Note that when listing the information, the last item should have
no following space.
This is an issue with how the PGF `foreach` works.
If listing on several lines (as in the example), put a comment
character after the last.

2. The coordinates can be specified in any TikZ way.

3. In designing this, I discovered a small bug in the `hobby` package
which this uses internally.
So for it to work correctly you should get the latest version of that
package from github.

4. The last coordinate on a shape should be a "join" coordinate.

5. There's a pre-designed `torus surface=n` for an `n,1` torus knot.

