
import pygame
import math
from pygame import mixer
import random

pygame.init()
def restartGame():
    # creating game window
    scr = pygame.display.set_mode((800, 600))



    background  = pygame.image.load("bg.png")

    # background sound

    mixer.music.load('background.wav')
    mixer.music.play(-1)
    # 


    # score 

    scoreValue = 0
    font = pygame.font.Font('freesansbold.ttf',32 )

    txtX = 10
    txtY = 10
    # 
    # game over text
    fontOver = pygame.font.Font('freesansbold.ttf',64)



    # 
    # caption and icon
    icon = pygame.image.load('ufo.png')
    pygame.display.set_caption("Space Invaders by VEDANT PARMANE")
    pygame.display.set_icon(icon)
    # 
    # enemy

    enemyImg = []
    enemyX = []
    enemyY = []
    enemyVelX= []
    enemyVelY = []



    noEnemy = 6

    for  i in range(noEnemy):




        enemyImg.append( pygame.image.load('enemy.png'))
        # enemyY.append( random.randint(25,150) )
        # enemyX.append( random.randint(0,735) )
        enemyX.append(random.randint(0, 736))
        enemyY.append(random.randint(50, 150))
        enemyVelX.append( 4 )
        enemyVelY.append( 40 )
        
    # 
    # player


    playerImg = pygame.image.load('plimg.png')
    playerX = 370
    playerY = 480
    playerVelX = 0


    # bullet
    bulletImg = pygame.image.load('bullet1.png')

    bulletY = 480
    bulletState = "ready"
    bulletVelY = 10
    bulletX = 0


    def showScore(x,y):

        
        score = font.render("Score : " + str(scoreValue)  ,True, (255,255,255))
        scr.blit(score,(x,y))

    def gameOverTxt():

        gametxtOver = fontOver.render("GAME OVER "   ,True, (255,255,255))
        scr.blit(gametxtOver,(200,250))




    running = True

    def player (x,y):
        scr.blit(playerImg,(x,y))

    def enemy (x,y,i): 
        scr.blit(enemyImg[i],(x,y))

    def bulletFire(x,y):
        global bulletState 
        bulletState = "fire"
        scr.blit(bulletImg,(x+16,y+10))

    def isColli(enemyX,enemyY,bulletX,bulletY):
        dist = math.sqrt((math.pow(enemyX - bulletX , 2)) + (math.pow (enemyY - bulletY,2 )))
        if dist < 27 :
            return True
        else :
            return False

    # main loop
    while running :
        '''enemyY += 0.1'''
        # adding background image
        scr.blit(background,(0,0))





        for event in pygame.event.get():

            if event.type == pygame.QUIT:
                running = False

            if event.type == pygame.KEYDOWN:

                if event.key == pygame.K_LEFT:

                    playerVelX = -5.5

                if event.key == pygame.K_RIGHT:
                    playerVelX = 5.5

                if event.key == pygame.K_SPACE:

                    if bulletState is "ready":

                        blsd = mixer.Sound('laser.wav')
                        blsd.play()

                        bulletX = playerX
                        bulletFire(bulletX,bulletY)



            if event.type == pygame.KEYUP:

                if event.key == pygame.K_LEFT or   event.key == pygame.K_RIGHT:
                    playerVelX = 0

        # checking for boundries        
        if playerX >= 736:
            playerX =  736
        
        if playerX <= 0:
            playerX = 0

        for i in range(noEnemy):

            if enemyY[i] > 440:

                for j in range(noEnemy):
                    enemyY[j] = 2000
                gameOverTxt ()
                break
            





            enemyX[i] = enemyX[i] + enemyVelX[i]
            
            # enemy movement
            if enemyX[i] >= 736:
                enemyVelX[i] = -4
                enemyY[i] += enemyVelY[i]
            
            if enemyX[i] <= 0:
                enemyVelX[i] = 4
                enemyY[i] += enemyVelY[i]

# collision
            collision = isColli(enemyX[i],enemyY[i],bulletX,bulletY)
            if collision :
                bulletY = 480
                bulletState = "ready"
                scoreValue += 10 
                
                # exsd = mixer.Sound('explosion.wav')
                exsd = mixer.Sound('explosion.wav')
                exsd.play()

                enemyX[i] = random.randint(0,735)
                enemyY[i] = random.randint(25,150)
                # noEnemy += 

            enemy(enemyX[i],enemyY[i],i)

        # bullet movement
        if bulletY <= 0:
            bulletY = 480
            bulletState = "ready"

        if bulletState == "fire" :

            bulletY -= bulletVelY
            bulletFire(bulletX,bulletY)
            



        
        playerX = playerX + playerVelX 


        
        player(playerX,playerY)

        showScore(txtX,txtY)
        
        pygame.display.update()


    pygame.quit()
    quit
restartGame()
