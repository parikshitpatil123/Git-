# Snake_Game
import pygame
import  random
pygame.mixer.init()  # song music
from pygame.examples.go_over_there import clock

pygame.init()

# Colours
white=(255,255,255)
red=(255,0,0)
black=(0,0,0)

# Creating Windows Size and Font
width=900
height=500
gameWindow= pygame.display.set_mode((width,height))

over= pygame.image.load("game_over.jpg")   # Image add
over=pygame.transform.scale(over,(width,height)).convert_alpha()

start= pygame.image.load("satart.jpg")
start=pygame.transform.scale(start,(width,height)).convert_alpha()

bgimg= pygame.image.load("m4.jpg")
bgimg=pygame.transform.scale(bgimg,(width,height)).convert_alpha()



# gam title
pygame.display.set_caption("Snake Game")
pygame.display.update()


# craete a clock
clock =pygame.time.Clock()

font=pygame.font.SysFont(None,55)


# Creating font on screen
font=pygame.font.SysFont(None,55)
def text_screen(text,color,x,y):
     screen_text=font.render(text,True,color)
     gameWindow.blit(screen_text,[x,y])

def plot_snake(gameWindow, color, snake_list,snake_size):
    for x,y in snake_list:
        pygame.draw.rect(gameWindow,color,[x,y,snake_size,snake_size])

def welcome():        # use for game over action
    exit_game= False
    while not exit_game:
        gameWindow.fill((233,220,229))
        gameWindow.blit(start, (0, 0))
        text_screen("<Welcome To Snake>",white,285,200)
        text_screen("Press Space Bar To Play",white,255,240)
        for event in pygame.event.get():
            if event.type==pygame.QUIT:
                exit_game=True
            if event.type==pygame.KEYDOWN:
                if event.key==pygame.K_SPACE:
                    pygame.mixer.music.load('c3dc_game_start_pra.mp3')
                    pygame.mixer.music.play()
                    gameloop()

        pygame.display.update()
        clock.tick(60)

def gameloop():
#Game specific variables
    exit_game= False
    game_over=False
    snake_x=40
    snake_y=55
    score=0
    init_velocity=5
    with open("HighScore.txt", "r") as f:
       hiscore = f.read()
    food_x=random.randint(10,width)
    food_y=random.randint(10,height)
    velocity_x=0
    velocity_y=0
    snake_size=30
    fps=60
    snake_list=[]
    snake_length=1


# Create a Game Loop
    while not exit_game:        # loop for run in continue
        if game_over:
            with open("HighScore.txt","w")as f:
                f.write(str(hiscore))
            gameWindow.fill(white)
            gameWindow.blit(over, (0, 0))
            text_screen("!!Press Enter to continue!!",red,225,333)

            for event in pygame.event.get():
                if event.type== pygame.QUIT:
                    exit_game= True
                if event.type==pygame.KEYDOWN:
                        if event.key==pygame.K_RETURN:
                            welcome()

        else:
            for event in pygame.event.get():
                if event.type== pygame.QUIT:
                    exit_game= True
                if event.type==pygame.KEYDOWN:
                    if event.key== pygame.K_RIGHT:
                     velocity_x=init_velocity
                     velocity_y=0

                    if event.key == pygame.K_LEFT:
                        velocity_x= - init_velocity
                        velocity_y = 0
                    if event.key == pygame.K_UP:
                        velocity_y= - init_velocity
                        velocity_x=0
                    if event.key == pygame.K_DOWN:
                        velocity_y= init_velocity
                        velocity_x = 0

                    if event.key== pygame.K_q:  # chat cheep
                        score+=10

            snake_x=snake_x+velocity_x
            snake_y=snake_y+velocity_y
            if abs(snake_x-food_x)<6 and abs(snake_y-food_y)<6:  # Adding Score
                score+=10
                print("Score:",score*10)
                food_x=random.randint(10,width)
                food_y=random.randint(10,height)
                snake_length +=5
                if score>int(hiscore):
                    hiscore=score

            gameWindow.fill(white)
            gameWindow.blit(bgimg, (0, 0))
            text_screen("Score"+str(score)+"  Highscore"+str(hiscore),red,5,5)
            pygame.draw.rect(gameWindow,red,[food_x,food_y,snake_size,snake_size])

            head = []
            head.append(snake_x)
            head.append(snake_y)
            snake_list.append(head)

            if len(snake_list) > snake_length:
                del snake_list[0]

            if head in snake_list[:-1]:
                game_over=True
                pygame.mixer.music.load('game_over.mp3')
                pygame.mixer.music.play()

            if snake_x<=0 or snake_x>width or snake_y<=0 or snake_y>height: # snake over
                game_over=True #print("Game Is Over")
                pygame.mixer.music.load('game_over.mp3')
                pygame.mixer.music.play()

            pygame.draw.rect(gameWindow,black,[snake_x,snake_y,snake_size,snake_size])
            plot_snake(gameWindow,black,snake_list, snake_size)

        pygame.display.update()
        clock.tick(fps)
    pygame.quit()
    quit()

welcome()

