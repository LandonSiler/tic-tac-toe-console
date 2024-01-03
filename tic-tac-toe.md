> ## Intro:

Javascript, is a programming language that is used for many things.

The language is a core piece of the internet; it controls web servers, it controls web pages, its the reason why the web is as capable as it is.

So to make use of and demonstrate JavaScripts abilities, we'll create a game of tic-tac-toe with it.

> #### About Tic-Tac-Toe:

The game tic-tac-toe is turn based, typically with two teams. These two teams usually contain no more than one member each;
in order to win during a turn, you must complete a full sequence with your teams marker.
These markers are typically `X`'s and `O`'s.
These markers can change as customization permits.
There are 3 sequence types that can be completed.
`Vertical, Horizontal, and Diagonal.`
Vertical sequences are grid columns, horizontal sequences are grid rows, and diagonal sequences are the two full length corner to corner arrays of tiles.

There are some important calculable properties in tic-tac-toe.

One such is the quantity of winnable sequences, this property of a tic-tac-toe game factors in the boards length in playable tiles. The number of winnable sequences is found in the equation `2b + 2` where b is the sqaure root of the total number of tiles, or put simply the height or width in tiles of the game board.
For a `2by2` grid this is `6`.
For a `3by3` grid this is `8`.
For a `4by4` grid this is `10`.
And so on.

Another calculable property is the minimum number of turns that can be taken to win the game, this minimum number is only achievable
by the first team to play a turn, and is found through the equation
`2b - 1`, where b is the boards height or width in tiles.

For example- the smallest a tic-tac-toe game board can be is `2by2`, and in such a configuration, the shortest number of turns you must take to win is `3`.
That being said with a `4by4` board the shortest number of turns that can be taken to win is `7`.
With a `5by5` the shortest number of turns is `9`, and so on.

> Transition ~queue~

When trying to interact with tic-tac-toe its important to recognize that Tic-tac-toe is a game; 
and like most human observations and innovations, is a concept that requires mediums to materialize.

So lets use mediums to materialize tic-tac-toe so we can play it!

> ### Using JavaScript To Build Tic-Tac-Toe:

To build tic-tac-toe, we can use code, more specifically JavaScript as a medium to manipulate electricity in such a way that we've materialized tic-tac-toe.

In bare bones vanilla Javascript; that being Javascript that has not been modified by libraries, interpreters or other tools, we really only have one 
safe way to visualize something on a screen, and thats via the console.

The console is an amazing way to relay data to developers, but it can also be used in creative ways to create imagery.

Lets get into creating the game board with the console.

> ## Using The Console To Visualize TTT:

Standard tic-tac-toe boards are a 3by3 grid.

And luckily for us, grids have been standardized by math.

Tic-tac-toe grids are biaxial grids, meaning they have two axes, we can label these axes the x and y axes.

The x axis is typically and will be in our case the horizontally parallel stretching axis and the y axis will be the vertically parallel stretching axis.

On these axes we know we need tiles, regardless of shape to be formed, in order to play tic-tac-toe.

Lucky us again, shapes are also defined by math.

We can use rectangles to avoid needing complex unicode characters later on, rectangles are shapes which must have four vertices, with all opposite lines being parallel, regardless of length.

What does this mean for our grid? It means that each line on an axis, in order to support rectangles must be parallel with other lines on the same axis and perpendicular to lines on the
opposite axis.

Now that we know this, its just a matter of creating the grid with text.

> ### Textual Grids:

Most environments that run Javascript, support UTF-8, which is a standard for converting numeric values to unicode characters.

This means, you can use any utf-8 supported unicode character to make a grid.

In our case we can use unicode character quantity, regardless of character width, as our unit of measurement, and utilize unicode vertical and horizontal bars to represent the location at which
the lines appear. 

> ### Grid Structure:

This is what our grid looks like:
```txt
 o | x | x
―――|―――|―――		
 x | x | x
―――|―――|―――
 x | x | o 
```
Take into mind that each space that holds a character is a tile.

In our example a tile will be three characters in width by one character in height.

Between each tile we use one character length, to separate them from each-other.

The separators filling that single character length we use are the unicode horizontal and vertical bar characters, mentioned before.

Please know, this math is important only for our visualization, when we perform math on tiles later for game logic, we will not need any of these measurements.
Instead we'll use the concept of tiles within our code to create a virtual grid, meaning all the extra separation and spacing characters won't be relevant for game logic.

> Transition ~queue~

Now that we have the grid, lets look at printing it in the console.

> ### Console Structuring:

We can print out our grid with multiple lines of console.log.
Or we can use a single line and employ a string feature... a new line.

In Javascript the way to create a new line, is to escape the character n.

To escape a character, you prepend a backslash.

Meaning to create a new line, you would use backslash n `\n`.

For example here's the code to print a `3by3` board using a singular line.

```js
console.log(`   |   |   \n―――|―――|―――\n   |   |   \n―――|―――|―――\n   |   |   `);
```

> Transition ~queue~

This works great if the board is static, meaning it wont change.
But this isn't the case, as we know we will have markers from each team, *likely x's and o's*
added in on each turn, and optionally a selector character; that may indicate which tile a team is selecting for their turn.

> ### Instantiable Strings:

In order to make our life easier, we can simplify our code with variables.
To do so, we can turn tiles and, rows containing tiles into instantiable strings, while turning separators and separator rows into constant strings.

The code for this in a `3by3` grid looks like this.

```js
function tile(character="") {
   return ` ${character} `;
}

const verticalSeparator = "|";
const horizontalSeparator = "―――";
const tileLine = (Tiles) => Tiles.join(verticalSeparator); 
const separatorLine = `${horizontalSeparator}${verticalSeparator}${horizontalSeparator}${verticalSeparator}${horizontalSeparator}`;
```
As a side note, we can make the tile function take up less space by taking advantage of arrow functions.

Here is our updated Tile function.

```js
const tile = (character="") => ` ${character} `;
```

This allows us todo the same amount of work with less code.

> Transition ~queue~

Now that we have the code capable of creating tiles as well as our constants;
what does this look like visually?

> ## Testing Our Visualization Code:

Lets print an example game board and verify that everything works.

The current code looks like this:

```js
const tile = (character = " ") => ` ${character ? character : " "} `;
const verticalSeparator = "|";
const horizontalSeparator = "―――";
const tileLine = (characters) => characters.map(c => tile(c)).join(verticalSeparator); 
const separatorLine = `${horizontalSeparator}${verticalSeparator}${horizontalSeparator}${verticalSeparator}${horizontalSeparator}`;

console.log(`${tileLine(["","",""])}\n${separatorLine}\n${tileLine(["","",""])}\n${separatorLine}\n${tileLine(["","",""])}`);
```

When ran, with this context we get our `3by3` game board.

To make our life easier, and make our code more connected, lets create variables to represent tiles that will be used in our board, these tile variables will
allow us to create our virtual grid to perform game logic, while also providing a way for the visualization to synchronize.

To do so we can create a standard `3by3` grid using three arrays with 3 characters in each of them, each array will represent a row, and each character will represent the value in the tile
at that location.

Our code for this alone, will look like this:

```js
const row0 = ["","",""];
const row1 = ["","",""];
const row2 = ["","",""];
```

If you start putting x's and o's in there you might notice our virtual game board!

As an example look at this snippet of code with pre-filled x's and o's.

```js
const row0 = ["x","o"," "]; // x | o | 
const row1 = [" ","x"," "]; //   | x | 
const row2 = ["o"," ","x"]; // o |   | x
```

While this method works...

We can also use a single array to store all of the tiles.
Along side of which we can specify our game board length in tiles, and perform math to allow scalability.

With this method, we could easily expand our game to a larger board like `4by4` or `5by5`.

This code would look like this:

```js
const boardSize = 3;
const defaultTileCharacter = " ";
const gameBoard = Array.from({length:boardSize**2}, () => defaultTileCharacter);
const getRow = (row) => gameBoard.slice(row*boardSize,(row+1)*boardSize);
```

The boardSize variable defines the size the board should be.

The defaultTileCharacter variable defines the default character in the board.

The gameBoard variable is an array representing the game board. Its value is an array, of the default character we specified, which has a length of the board size variable squared.

The reason this is, is because the board size squared is exactly how many tiles we have. For a 3by3 game we have 3 squared, or 9 tiles. For a 4by4 game we have 4 squared, or 16 tiles, and so on.

The getRow function uses the previous variables to get the row specified by the row parameter, it uses the gameBoard array and the boardSize in combination with the requested row to find the
indexes at which the requested row sits in the array, and then returns them in an array representing just that row.

Now that we have our game logic set up to scale, we can use these variables to scale our visualization.

Lets take a look at how we can do this.

> ### Scaling Visuals

Since most of our visualization code was already set up to scale, all we need to do is change the separatorLine variable and the final game board string.

Right now the separator is hardcoded to only have three horizontal separators.

All we need to do, is change it to factor in the boardSize.

Here's what that looks like:

```js
const separatorLine = `${"".padStart((`${horizontalSeparator}${verticalSeparator}`.length)*(boardSize-1), `${horizontalSeparator}${verticalSeparator}`)}${horizontalSeparator}`;
```

The way it operates is by using a string function.
This string function is known as padStart, pad start creates the given pattern in the string from index 0
up until the specified index.

Since our pattern ends with a vertical separator, and we need it to end with a horizontal separator, we'll end the pattern one iteration before the number of pattern instances
we actually need.
After which we'll append the final horizontal separator.

This is to ensure the game board separators spanning the x axis always starts and ends with a horizontal separator.

> Transition ~queue~

Knowing the separatorLines updates,
what does the rendered board string look like?

> ### Rendered Board String:

The following is what the rendered board string looks like.

```js
const renderedBoard = Array.from({length:boardSize}, (v,i) => tileLine(getRow(i))).join("\n"+separatorLine+"\n");
```

The rendered board string uses an array to begin its construction, the array is the created with the length of the board in tiles, where
each of the items within this array are ultimately tile rows, 
that are parsed from the gameBoard array via getRow.

We create the final string by using join on the array of tile rows, join takes each tile row item and adds a separator row on a new line directly after, as well as appending another new line. 

This ultimately allowing the subsequent tile row to be placed on a new line after the joined separator row.

Lets take a look at our visualization and game logic code together now:

```js
const boardSize = 3;
const defaultTileCharacter = " ";
const gameBoard = Array.from({length:boardSize**2}, () => defaultTileCharacter);
const getRow = (row) => gameBoard.slice(row*boardSize,(row+1)*boardSize);

const tile = (character = " ") => ` ${character ? character : " "} `;
const verticalSeparator = "|";
const horizontalSeparator = "―――";
const tileLine = (characters) => characters.map(c => tile(c)).join(verticalSeparator); 
const separatorLine = `${"".padStart((`${horizontalSeparator}${verticalSeparator}`.length)*(boardSize-1), `${horizontalSeparator}${verticalSeparator}`)}${horizontalSeparator}`;
const gameRender = Array.from({length:boardSize}, (v,i) => tileLine(getRow(i))).join("\n"+separatorLine+"\n");

console.log(gameRender)
```

to be continued...
