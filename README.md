# Final-Project-code
import turtle
import time
import random

delay = 0.1

score = 0
high_score = 0

screen = turtle.Screen()
screen.title("Snake Game")
screen.bgcolor("skyblue")
screen.setup(width=600, height=600)
screen.tracer(0)

snake = turtle.Turtle()
snake.speed(0)
snake.shape("square")
snake.color("black")
snake.penup()
snake.goto(0,0)
snake.direction = "stop"

food = turtle.Turtle()
food.speed(0)
food.shape("square")
food.color("red")
food.penup()
food.goto(0,100)

tails = []

scoring = turtle.Turtle()
scoring.speed(0)
scoring.shape("square")
scoring.color("white")
scoring.penup()
scoring.hideturtle()
scoring.goto(0, 260)
scoring.write("Score: 0  High Score: 0", align="center", font=("Courier", 24, "normal"))


def go_up():
    if snake.direction != "down":
        snake.direction = "up"

def go_down():
    if snake.direction != "up":
        snake.direction = "down"

def go_left():
    if snake.direction != "right":
        snake.direction = "left"

def go_right():
    if snake.direction != "left":
        snake.direction = "right"

def move():
    if snake.direction == "up":
        y = snake.ycor()
        snake.sety(y + 20)

    if snake.direction == "down":
        y = snake.ycor()
        snake.sety(y - 20)
    if snake.direction == "left":
        x = snake.xcor()
        snake.setx(x - 20)

    if snake.direction == "right":
        x = snake.xcor()
        snake.setx(x + 20)

screen.listen()
screen.onkeypress(go_up, "w")
screen.onkeypress(go_down, "s")
screen.onkeypress(go_left, "a")
screen.onkeypress(go_right, "d")

while True:
    screen.update()

    if snake.xcor() > 290 or snake.xcor() < -290 or snake.ycor() > 290 or snake.ycor() < -290:
        time.sleep(1)
        snake.goto(0,0)
        snake.direction = "stop"

        for tail in tails:
            tail.goto(1000, 1000)
        
        tails.clear()

        score = 0

        delay = 0.1

        scoring.clear()
        scoring.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal")) 


    if snake.distance(food) < 20:
        x = random.randint(-290, 290)
        y = random.randint(-290, 290)
        food.goto(x,y)

        new_tail = turtle.Turtle()
        new_tail.speed(0)
        new_tail.shape("square")
        new_tail.color("yellow")
        new_tail.penup()
        tails.append(new_tail)

        delay -= 0.001

        score += 10

        if score > high_score:
            high_score = score
        
        scoring.clear()
        scoring.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))

    for index in range(len(tails)-1, 0, -1):
        x = tails[index-1].xcor()
        y = tails[index-1].ycor()
        tails[index].goto(x, y)

    if len(tails) > 0:
        x = snake.xcor()
        y = snake.ycor()
        tails[0].goto(x,y)

    move()
    
    for tail in tails:
        if tail.distance(snake) < 20:
            time.sleep(1)
            snake.goto(0,0)
            snake.direction = "stop"
        
            for tail in tails:
                tail.goto(1000, 1000)
        
            tails.clear()

            score = 0

            delay = 0.1
        
            scoring.clear()
            scoring.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))

    time.sleep(delay)

screen.mainloop()
