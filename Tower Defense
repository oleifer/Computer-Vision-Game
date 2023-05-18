
import cv2 
import random
from cvzone.HandTrackingModule import HandDetector

height = 720
width = 1280

max_Balls = 30
max_Wall = 50
max_Players = 2

# create the lists you need
players = []
balls  = []
bullets = []
wall = []  

# variables
total_Score = 0
game_over = False

# text info
# font
font = cv2.FONT_HERSHEY_SIMPLEX  


#######################################################################
## ball enemy
class Ball:
    def __init__(self):

        # ball location
        x_pos = random.randint(50, width-50)
        y_pos = random.randint(-100, -10)
        # ball speed
        speed_mag = 35
        x_speed = random.randint(-speed_mag, speed_mag)
        y_speed = random.randint(-speed_mag, speed_mag)

        # color
        blue  = random.randint(0, 255)
        green = 0
        red  = random.randint(100, 255)

        self.center = [x_pos, y_pos]
        self.radius = 20
        self.color = [blue, green, red] # BGR color
        self.thickness = -1 
        self.speed = [x_speed, y_speed]
        self.die = False

    def update(self):
            
        self.draw()
        self.barrier()
        self.move()
        self.invade()
        self.is_offscreen()

    def draw(self):
        cv2.circle(image, self.center, self.radius + 3, (0, 0, 0), self.thickness)
        cv2.circle(image, self.center, self.radius, self.color, self.thickness)

    def barrier(self):

        # now we need to tell the ball to bounce off the walls
        # top bottom walls
        if self.center[1]<= -150:
            self.speed[1] = -self.speed[1]

        # side wall
        if self.center[0]<= 50 or self.center[0]>= width-50:
            self.speed[0] = -self.speed[0]
    
    def invade(self):
        global game_over 
                # if you pass through the game ends
        if self.center[1]>= height:
            game_over = True

    # remove the ball if it's offscreen
    def is_offscreen(self):
        if self.center[1] > height + 50:  # check if the ball is offscreen
            self.die = True

    def move(self):

        # move Ball
        self.center[0] = self.center[0] + self.speed[0]
        self.center[1] = self.center[1] + self.speed[1]
# if ball is hit, kill it and update score
    def hit(self):
        global total_Score
        self.die = True
        total_Score += 1

    # check if ball has hit other object
    def has_hit(self, other_object):

        distance = ((self.center[0] - other_object.center[0])**2 + (self.center[1] - other_object.center[1])**2)**0.5
        # Check if the distance is less than the sum of their radii
        if distance < self.radius + other_object.radius:
            self.die = True
            return True
        else:
            return False


#######################################################################

class Wall:
    def __init__(self, x_pos, y_pos):

        # color
        blue  = random.randint(100, 255)
        green = random.randint(0, 255)
        red  = 0

        self.center = [x_pos, y_pos]
        self.radius = 30
        self.color = [blue, green, red] # BGR color
        self.thickness = -1 
        self.die = False

    def update(self):
            
        self.draw()


    def draw(self):
        cv2.circle(image, self.center, self.radius + 3, (0, 0, 0), self.thickness)
        cv2.circle(image, self.center, self.radius, self.color, self.thickness)


    def hit(self):
        self.die = True


################################################################################
class Bullet:
    def __init__(self, x_pos, y_pos):


        # color
        blue  = 0
        green = random.randint(200, 255)
        red  = random.randint(200, 255)

        self.center = [x_pos, y_pos]
        self.radius = 5
        self.color = [blue, green, red] # BGR color
        self.thickness = -1 
        self.speed = 15
        self.die = False

    def update(self):
            
        self.draw()
        self.move()
        self.is_offscreen()

    def draw(self):
        cv2.circle(image, self.center, self.radius + 2, (0, 0, 0), self.thickness)
        cv2.circle(image, self.center, self.radius, self.color, self.thickness)

    def move(self):
        self.center[1] = self.center[1] - self.speed  # move the bullet upwards

    # remove the bullet if it's offscreen
    def is_offscreen(self):
        if self.center[1] < -200:  # check if the bullet is offscreen
            bullets.remove(bullet)

    def has_hit(self, other_object):

        distance = ((self.center[0] - other_object.center[0])**2 + (self.center[1] - other_object.center[1])**2)**0.5
        # Check if the distance is less than the sum of their radii
        if distance < self.radius + other_object.radius:
            self.die = True
            return True
        else:
            return False

        # check if the bullet has hit the other object
        # return True or False depending on whether it has hit

################################################################################
class Player:
    def __init__(self, x_pos):

        self.center = [x_pos, height-50]
        self.radius = 50
        self.color = [0, 0, 0] # BGR color
        self.thickness = -1 

    def update(self):
            
        self.draw()

    def draw(self):
        cv2.circle(image, self.center, self.radius, self.color, self.thickness)


########################################################################

def shooting(x_pos):

    # add new bullets when the player shoots
    
    new_bullet = Bullet(x_pos, height-100)
    bullets.append(new_bullet)

###########################################################
def check_for_hits():

    # update all the bullets
    for bullet in bullets:

        # check if the bullet has hit any other objects
        for ball in balls :
            if bullet.has_hit(ball):
                ball.hit() 

            # remove the ball if it hits object
            if ball.die:
                balls.remove(ball)
                

         # remove the bullet if it hits object
        if bullet.die:
            bullets.remove(bullet)

    # check if ball has hit the wall
    for ball in balls:

        for brick in wall:

            if ball.has_hit(brick):
                brick.hit()

            if brick.die:
                wall.remove(brick)

#################################################################
def give_birth():

    current = len(balls)

    newBalls = max_Balls - current


    for i in range(newBalls):
        newBall = Ball()
        balls.append(newBall)

######################################################
def check_death():

    for ball in balls :
        if ball.die:
            balls.remove(ball)
###########################################################

## initialise objects
def initialise():

    # empty all the lists + variables
    global players 
    global balls  
    global bullets 
    global wall 
    global total_Score
    global game_over

    players = []
    balls  = []
    bullets = []
    wall = []  
    total_Score = 0 
    game_over = False

    for i in range(max_Balls):
        newBall = Ball()
        balls.append(newBall)

    brick_spacing = 40
    # loop to create and position objects
    # Loop through each row of the grid
    for row in range(6):
        # Loop through each column of the grid
        for col in range(35):
            # Calculate the x and y position of the object
            x_pos = col * brick_spacing
            y_pos = height - (row * brick_spacing) 
            
            # Create the object and add it to the list of objects
            newWall = Wall(x_pos, y_pos)
            wall.append(newWall)


    init_x = width//(max_Players+1)

    for i in range(max_Players):
        
        newPlayer = Player(init_x)
        players.append(newPlayer)
        init_x += init_x
        



####################################################################

cap = cv2.VideoCapture(0)

cap.set(3, width) 
cap.set(4, height)

# hand detection
detector = HandDetector(detectionCon=0.8, maxHands=2, minTrackCon=0.5)

initialise()


while cap.isOpened():


    ret, frame = cap.read()
    # flip image
    image = cv2.flip(frame, 1)
    

    # this returns a dictionary of all the hands detected
    hands, image = detector.findHands(image, flipType=False)


    # update objects
    for ball in balls:
        ball.update()
       
    for brick in wall:
        brick.update()
    
    for player in players:
        player.update()

    # update all the bullets
    for bullet in bullets:
        bullet.update()

    # check bullets on screen
    check_for_hits()
    # if balls are destroyed make new ones
    give_birth()
    check_death()

    # write info on screen
    score = f"Score : {total_Score}"
    cv2.putText(image, score, (50, 50), font, 
                   1, (0, 0, 0), 8)
    cv2.putText(image, score, (50, 50), font, 
                   1, (255, 0, 0), 2)
    
    if (game_over):
        cv2.putText(image, "GAME OVER", (170, 400), font, 
                   5, (0, 0, 0), 30)
        cv2.putText(image, "GAME OVER", (170, 400), font, 
                   5, (0, 0, 255), 15)

    # check for hands
    if hands and not game_over:
        for hand in hands:

            if hand["type"] == "Left":

                lefty = hand['bbox']# Bounding box info x,y,w,h
                players[0].center[0] = lefty[0]

                fingers = detector.fingersUp(hand)

                if fingers[0] == 1 :
                    shooting(lefty[0])


            
            if hand["type"] == "Right":

                righty = hand['bbox']
                players[1].center[0] = righty[0]
                                
                fingers = detector.fingersUp(hand)

                if fingers[0] == 1 :
                    shooting(righty[0])


    cv2.imshow('Game Window', image)

    # restart
    if cv2.waitKey(1) & 0xFF == ord('r'):
        initialise()

    # quit
    if cv2.waitKey(10) & 0xFF == ord('q'):
        break

    # close screen
    if cv2.getWindowProperty('Game Window', cv2.WND_PROP_VISIBLE) < 1:
        break


cap.release() 
cv2.destroyAllWindows() 
