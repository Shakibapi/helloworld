# helloworldimport turtle
import random
import time


# return second
def time_second():
    time.localtime()
    s = time.localtime().tm_sec
    return s


# تنظیمات صحنه
win = turtle.Screen()
win.title("لاکپشت و توپ")
win.bgpic("9.gif")
win.setup(width=550, height=600)
win.tracer(0)

# ایجاد لاکپشت
lak = turtle.Turtle()
lak.shape("turtle")
lak.color("cyan")
lak.penup()

# ایجاد توپ‌ها
balls = []
num_balls = 1

w = turtle.Turtle()
w.color("black")
w.speed(10)
w.up()
w.goto(-258, 255)
w.down()
w.width(4)
for i in range(4):
    w.forward(515)
    w.right(90)
w.ht()


def create_ball():
    ball = turtle.Turtle()
    ball.shape("circle")
    ball.color(random.choice(["red", "blue", "green", "yellow", "purple", "orange"]))  # رنگ تصادفی انتخاب می‌شود
    ball.penup()
    ball.goto(0, 0)  # توپ‌ها از وسط صحنه ظاهر می‌شوند
    ball.goto(random.randint(-100, 100), random.randint(-100, 100))  # توپ‌ها در محدوده اطراف وسط ظاهر می‌شوند
    balls.append(ball)


# امتیاز
score = 0

# نمایش امتیاز روی صفحه
am = turtle.Turtle()
am.speed(0)
am.color("black")
am.penup()
am.hideturtle()
am.goto(0, 260)
am.write("جایزه: {}".format(score), align="center", font=("Courier", 24, "normal"))


# تابع بررسی برخورد لاکپشت و توپ
def is_collision(t1, t2):
    distance = t1.distance(t2)
    if distance < 20:
        return True
    else:
        return False


# حرکت لاکپشت
def move_up():
    y = lak.ycor()
    y += 20
    lak.sety(y)
    lak.setheading(90)  # تنظیم جهت صورت لاکپشت به بالا

def move_down():
    y = lak.ycor()
    y -= 20
    lak.sety(y)
    lak.setheading(270)  # تنظیم جهت صورت لاکپشت به پایین

def move_left():
    x = lak.xcor()
    x -= 20
    lak.setx(x)
    lak.setheading(180)  # تنظیم جهت صورت لاکپشت به چپ

def move_right():
    x = lak.xcor()
    x += 20
    lak.setx(x)
    lak.setheading(0)  # تنظیم جهت صورت لاکپشت به راست



# اتصال حرکت به کلیدها
win.listen()
win.onkeypress(move_up, "Up")
win.onkeypress(move_down, "Down")
win.onkeypress(move_left, "Left")
win.onkeypress(move_right, "Right")

# حلقه اصلی بازی
count = 0
s1 = time_second()
while True:
    tx = int(lak.xcor())
    ty = int(lak.ycor())
    if tx > 248 or tx < -248 or ty > 220 or ty < -278:
        score -= 1
        am.clear()  # پاک کردن
        am.write("امتیاز: {}".format(score), align="center", font=("Courier", 24, "normal"))
        if tx > 248:
            lak.setx(lak.xcor() - 50)
        if tx < -248:
            lak.setx(lak.xcor() + 50)
        if ty > 220:
            lak.sety(lak.ycor() - 50)
        if ty < -278:
            lak.sety(lak.ycor() + 50)
    s2 = time_second()
    if s1 != s2:
        count += 1
        s1 = s2
    if count == 4:
        count = 0
        create_ball()
    win.update()
    for ball in balls:
        if (
            ball.xcor() > 248
            or ball.xcor() < -248
            or ball.ycor() > 220
            or ball.ycor() < -278
        ):
            ball.hideturtle()
            balls.remove(ball)#turquois
        else:
            ball.setx(ball.xcor() - 0.5)  # حرکت توپ به سمت لاکپشت
            # بررسی برخورد لاکپشت و توپ
            if is_collision(lak, ball):
                ball.goto(random.randint(-230, 230), random.randint(-270, 200))
                score += 1  # افزایش امتیاز
                am.clear()  # پاک کردن
                am.write("امتیاز: {}".format(score), align="center", font=("Courier", 24, "normal"))
