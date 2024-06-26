#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <time.h>
#include <windows.h>

// Defining the screen size
#define width 20
#define height 20

// Defining the directions
#define UP 1
#define DOWN 2
#define LEFT 3
#define RIGHT 4

// Global variables
bool gameover;
int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100];
int nTail;
int direction;

// Function to set up the initial state of the game
void setup()
{
    gameover = false;
    direction = RIGHT;
    x = width / 2;
    y = height / 2;
    fruitX = rand() % width;
    fruitY = rand() % height;
    score = 0;
}

// Function to draw the game screen
void draw()
{
    system("cls"); // Clear the screen

    // Print the top border
    for (int i = 0; i < width + 2; i++)
        printf("#");
    printf("\n");

    // Print the game area
    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            if (j == 0)
                printf("#"); // Print the left border

            if (i == y && j == x)
                printf("O"); // Print the snake's head
            else if (i == fruitY && j == fruitX)
                printf("F"); // Print the fruit
            else
            {
                bool printTail = false;
                for (int k = 0; k < nTail; k++)
                {
                    if (tailX[k] == j && tailY[k] == i)
                    {
                        printf("o"); // Print the snake's tail
                        printTail = true;
                    }
                }
                if (!printTail)
                    printf(" "); // Print an empty space inside the game area
            }

            if (j == width - 1)
                printf("#"); // Print the right border
        }
        printf("\n");
    }

    // Print the bottom border
    for (int i = 0; i < width + 2; i++)
        printf("#");
    printf("\n");

    printf("Score: %d\n", score); // Print the score
}

// Function to handle user input
void input()
{
    if (_kbhit()) // Check if a key has been pressed
    {
        switch (_getch()) // Get the pressed key
        {
        case 'w':
            if (direction != DOWN)
                direction = UP;
            break;
        case 's':
            if (direction != UP)
                direction = DOWN;
            break;
        case 'a':
            if (direction != RIGHT)
                direction = LEFT;
            break;
        case 'd':
            if (direction != LEFT)
                direction = RIGHT;
            break;
        case 'x':
            gameover = true;
            break;
        }
    }
}

// Function to update the game state
void logic()
{
    // Move the tail
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < nTail; i++)
    {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    // Move the snake's head
    switch (direction)
    {
    case UP:
        y--;
        break;
    case DOWN:
        y++;
        break;
    case LEFT:
        x--;
        break;
    case RIGHT:
        x++;
        break;
    }

    // Check for collision with walls
    if (x >= width || x < 0 || y >= height || y < 0)
        gameover = true;

    // Check for collision with tail
    for (int i = 0; i < nTail; i++)
    {
        if (tailX[i] == x && tailY[i] == y)
            gameover = true;
    }

    // Check if the snake eats the fruit
    if (x == fruitX && y == fruitY)
    {
        score += 10;
        fruitX = rand() % width;
        fruitY = rand() % height;
        nTail++;
    }
}

int main()
{
    srand(time(NULL)); // Seed for random number generation

    setup(); // Set up initial state of the game

    while (!gameover)
    {
        draw();  // Draw the game screen
        input(); // Handle user input
        logic(); // Update the game state
        Sleep(100); // Sleep for a while to control the speed of the game
    }

    printf("\nGame Over!\n");

    return 0;
}

Kishorevimal/Kishorevimal is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
