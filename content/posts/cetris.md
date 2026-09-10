+++
date = '2026-02-23T16:59:04+03:00'
draft = false
title = 'Cetris'
+++

![Screenshot from Cetris](/img/cetris/cetris.png)

**[Cetris](https://gitlab.com/den.ege.der/cetris)** is a Tetris implementation written in C. 
The project introduced me to several interesting challenges, including piece rotation, collision 
detection, terminal rendering, and randomization. Piece rotation follows the Super Rotation 
System (SRS), while the pieces are selected using a 7-bag randomizer, ensuring that every tetromino 
appears once before the sequence repeats.

## Working with Terminal Colors

One of the more difficult parts of the project was rendering the pieces with distinct colors. The 
terminal I was using supported only the standard eight ANSI colors. After reserving white for the 
borders and black for the background, there were not enough colors left for all seven tetrominoes.

I experimented with different ANSI escape sequences, but the color limitation initially appeared 
to be a hard constraint. The alternatives were either switching to a more capable terminal or 
simplifying the game’s appearance.

## Doubling the Color Range

The solution was to combine bold text with reversed foreground and background colors. Bold text can 
produce a brighter version of a terminal color. Since the tetrominoes were rendered using colored 
backgrounds, I first needed to make the color available as foreground text. The ANSI reverse attribute 
swaps the foreground and background colors, allowing the background color to be bolded. This produced 
two visually distinct shades from each base color:

- The normal background color
- A brighter version created with bold and reverse attributes

As a result, the eight available terminal colors could be rendered as sixteen effective variants, enough 
for the seven tetrominoes and the board border. For more information, 
[check the wikipedia page](https://en.wikipedia.org/wiki/ANSI_escape_code#3-bit_and_4-bit) The following 
program demonstrates the technique using `ncurses`. Compile it with the `-lncurses` flag:

```c
#include <ncurses.h>

// Pair index, base color,
// bold+reverse flag
static const struct {
    int       pair;
    int       fg;
    int       bold\_reverse;
} COLORS\_8x2[16] = {
    /* row 0: plain bg */
    { 1, COLOR\_CYAN,    0 },
    { 2, COLOR\_BLUE,    0 },
    { 3, COLOR\_YELLOW,  0 }, 
    { 4, COLOR\_GREEN,   0 },
    { 5, COLOR\_MAGENTA, 0 },
    { 6, COLOR\_RED,     0 },
    { 7, COLOR\_WHITE,   0 },
    { 8, COLOR\_BLACK,   0 },
    /* row 1: bold+reverse */
    { 1, COLOR\_CYAN,    1 },
    { 2, COLOR\_BLUE,    1 },
    { 3, COLOR\_YELLOW,  1 }, 
    { 4, COLOR\_GREEN,   1 },
    { 5, COLOR\_MAGENTA, 1 },
    { 6, COLOR\_RED,     1 },
    { 7, COLOR\_WHITE,   1 },
    { 8, COLOR\_BLACK,   1 },
};

int main(void)
{
    initscr();
    start\_color();
    cbreak();
    noecho();

    init\_pair(1, COLOR\_CYAN,    COLOR\_CYAN);
    init\_pair(2, COLOR\_BLUE,    COLOR\_BLUE);
    init\_pair(3, COLOR\_YELLOW,  COLOR\_YELLOW);
    init\_pair(4, COLOR\_GREEN,   COLOR\_GREEN);
    init\_pair(5, COLOR\_MAGENTA, COLOR\_MAGENTA);
    init\_pair(6, COLOR\_RED,     COLOR\_RED);
    init\_pair(7, COLOR\_WHITE,   COLOR\_WHITE);
    init\_pair(8, COLOR\_BLACK,   COLOR\_BLACK);

    for (int i = 0; i < 16; i++) {
        int col = (i % 8) * 4;
        int row = (i / 8) * 2;

        if (COLORS\_8x2[i].bold\_reverse)
            attron(A\_BOLD | A\_REVERSE);

        attron(COLOR\_PAIR(COLORS\_8x2[i].pair));

        mvprintw(row,     col, "    ");
        mvprintw(row + 1, col, "    ");

        attroff(COLOR\_PAIR(COLORS\_8x2[i].pair));
        attroff(A\_BOLD | A\_REVERSE);
    }

    mvprintw(5, 0, "Press any key to exit...");
    refresh();
    getch();
    endwin();

    return 0;
}
```

![Color Matrix](/img/cetris/color-matrix.png)
