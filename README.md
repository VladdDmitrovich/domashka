# domashka
#створи гру "Лабіринт"!
from pygame import *

window = display.set_mode((700, 500))
display.set_caption("Лабіринт")

background = transform.scale(image.load('background.jpg'), (700, 500))
kotik = transform.scale(image.load('kotik.png'), (700, 500))
kit = transform.scale(image.load('kit.png'), (700, 500))
clock = time.Clock()
start = True

mixer.init()
mixer.music.load('jungles.ogg')
mixer.music.play()

money = mixer.Sound('money.ogg')
kick = mixer.Sound('kick.ogg')

class GameSprite(sprite.Sprite):
    def __init__(self, name, x, y, width, height, speed):
        self.image = transform.scale(image.load(name), (width, height))
        self.speed = speed
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def move(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed

        if keys[K_DOWN] and self.rect.y < 435:
            self.rect.y += self.speed

        if keys[K_RIGHT] and self.rect.x < 635:
            self.rect.x += self.speed

        if keys[K_LEFT] and self.rect.x > 5:
            self.rect.x -= self.speed

class Enemy(GameSprite):
    ok = 'left'
    def move(self):
        if self.rect.x < 450:
            self.ok = 'right'
        if self.rect.x > 650:
            self.ok = 'left'


        if self.ok == 'left':
            self.rect.x -= self.speed
        else:
            self.rect.x += self.speed

class Stina(sprite.Sprite):
    def __init__(self, width, height, x, y, color):
        super().__init__()
        self.color = color
        self.width = width
        self.height = height
        self.image = Surface((self.width, self.height))
        self.image.fill(color)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
        

player = Player('hero.png', 50, 400, 70, 70, 5)

enemy = Enemy('cyborg.png', 600, 250, 60, 60, 5)

gold = GameSprite('treasure.png', 600, 420, 60, 60, 0)

stina1 = Stina(10, 400, 25, 10, (17, 122, 5))
stina2 = Stina(190, 10, 35, 10, (17, 122, 5))
stina3 = Stina(10, 190, 220, 10, (17, 122, 5))
stina4 = Stina(10, 300, 130, 200, (17, 122, 5))
stina5 = Stina(10, 200, 220, 200, (17, 122, 5))
stina7 = Stina(200, 10, 230, 390, (17, 122, 5))
stina8 = Stina(10, 200, 510, 300, (17, 122, 5))
stina9 = Stina(10, 300, 130, 200, (17, 122, 5))
stina10 = Stina(210, 10, 300, 300, (17, 122, 5))
stina11 = Stina(10, 200, 440, 100, (17, 122, 5))
stina12 = Stina(70, 10, 300, 200, (17, 122, 5))
stina13 = Stina(500, 10, 200, 10, (17, 122, 5))
stina14 = Stina(310, 10, 300, 100, (17, 122, 5))
stina15 = Stina(10, 100, 300, 100, (17, 122, 5))

font.init()
font1 = font.SysFont('Pasifico', 70)
win = font1.render("WINNER", True, (255, 0, 111))
lose = font1.render("LOSER", True, (255, 0, 0))

finish = False

while start == True:
    for e in event.get():
        if e.type == QUIT:
            start = False

    if finish == False:
        window.blit(background, (0, 0))


        player.reset()
        enemy.reset()
        gold.reset()
        stina1.reset()
        stina2.reset()
        stina3.reset()
        stina4.reset()
        stina5.reset()
        stina7.reset()
        stina8.reset()
        stina9.reset()
        stina10.reset()
        stina11.reset()
        stina12.reset()
        stina13.reset()
        stina14.reset()

        player.move()
        enemy.move()

        if sprite.collide_rect(player, gold):
            finish = True
            window.blit(kit, (0, 0))
            window.blit(win, (300, 250))
            money.play()


        if sprite.collide_rect(player, enemy) or sprite.collide_rect(player, stina1) or sprite.collide_rect(player, stina2) or sprite.collide_rect(player, stina3) or sprite.collide_rect(player, stina4) or sprite.collide_rect(player, stina5) or sprite.collide_rect(player, stina7) or sprite.collide_rect(player, stina8) or sprite.collide_rect(player, stina9) or sprite.collide_rect(player, stina10) or sprite.collide_rect(player, stina11) or sprite.collide_rect(player, stina12) or sprite.collide_rect(player, stina13) or sprite.collide_rect(player, stina14):
            finish = True
            window.blit(kotik, (0, 0))
            window.blit(lose, (300, 250))
            kick.play()
        

    display.update()
    clock.tick(60)
