import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
PLAYER_WIDTH, PLAYER_HEIGHT = 30, 30  # Reduced size
ENEMY_WIDTH, ENEMY_HEIGHT = 30, 30  # Reduced size
ITEM_WIDTH, ITEM_HEIGHT = 20, 20
BULLET_WIDTH, BULLET_HEIGHT = 5, 5
SPEED = 5
BULLET_SPEED = 10
ENEMY_BULLET_SPEED = 3
ENEMY_ITEM_SPEED = 3
ENEMY_SPAWN_INTERVAL = 120  # Frames (2 seconds at 60 FPS)
ENEMY_SHOOT_INTERVAL = 240   # Frames (4 seconds at 60 FPS)

# Load sound effects
ENEMY_DEATH_SOUND_PATH = 'Enemy_die.mp3'  # Path to your enemy death sound
ENEMY_SHOOT_SOUND_PATH = 'Bullet.mp3'  # Path to your enemy shoot sound
GAME_OVER_SOUND_PATH = 'Game Over.mp3'  # Path to your game over sound
VICTORY_SOUND_PATH = 'Victory.mp3'  # Path to your victory sound
BACKGROUND_MUSIC_PATH = 'Zombie_bg_music.mp3'  # Path to your background music

# Load sounds
enemy_death_sound = pygame.mixer.Sound(ENEMY_DEATH_SOUND_PATH)
enemy_shoot_sound = pygame.mixer.Sound(ENEMY_SHOOT_SOUND_PATH)
game_over_sound = pygame.mixer.Sound(GAME_OVER_SOUND_PATH)
victory_sound = pygame.mixer.Sound(VICTORY_SOUND_PATH)

# Load and play background music
pygame.mixer.music.load(BACKGROUND_MUSIC_PATH)
pygame.mixer.music.play(-1)  # Loop the music indefinitely

# Load images
PLAYER_IMAGE_PATH = 'sunflower.png'  # Path to your player image
ENEMY_TYPE_A_IMAGE_PATH = 'zombie1.png'  # Path to enemy type A image
ENEMY_TYPE_B_IMAGE_PATH = 'zombie2.png'  # Path to enemy type B image
ENEMY_TYPE_C_IMAGE_PATH = 'zombie3.png'  # Path to enemy type C image
BACKGROUND_IMAGE_PATH = 'zombie_bg.jpg'  # Path to your background image

# Load images
player_image = pygame.image.load(PLAYER_IMAGE_PATH)
player_image = pygame.transform.scale(player_image, (PLAYER_WIDTH, PLAYER_HEIGHT))
enemy_type_a_image = pygame.image.load(ENEMY_TYPE_A_IMAGE_PATH)
enemy_type_a_image = pygame.transform.scale(enemy_type_a_image, (ENEMY_WIDTH, ENEMY_HEIGHT))
enemy_type_b_image = pygame.image.load(ENEMY_TYPE_B_IMAGE_PATH)
enemy_type_b_image = pygame.transform.scale(enemy_type_b_image, (ENEMY_WIDTH, ENEMY_HEIGHT))
enemy_type_c_image = pygame.image.load(ENEMY_TYPE_C_IMAGE_PATH)
enemy_type_c_image = pygame.transform.scale(enemy_type_c_image, (ENEMY_WIDTH, ENEMY_HEIGHT))
background_image = pygame.image.load(BACKGROUND_IMAGE_PATH)  # Load the background image
background_image = pygame.transform.scale(background_image, (WIDTH, HEIGHT))  # Scale to fit screen

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Player vs Enemy")

# Player class
class Player:
    def __init__(self):
        self.rect = pygame.Rect(WIDTH // 2, HEIGHT - PLAYER_HEIGHT - 10, PLAYER_WIDTH, PLAYER_HEIGHT)

    def move(self, dx):
        self.rect.x += dx
        self.rect.x = max(0, min(self.rect.x, WIDTH - PLAYER_WIDTH))

    def draw(self, surface):
        surface.blit(player_image, self.rect.topleft)

# Base Enemy class
class Enemy:
    def __init__(self):
        self.rect = pygame.Rect(random.randint(0, WIDTH - ENEMY_WIDTH), random.randint(50, 150), ENEMY_WIDTH, ENEMY_HEIGHT)

    def throw_item(self):
        return Item(self.rect.centerx)

    def shoot(self):
        enemy_shoot_sound.play()  # Play shoot sound
        return EnemyBullet(self.rect.centerx, self.rect.bottom)

# Different Enemy Types
class EnemyTypeA(Enemy):
    def draw(self, surface):
        surface.blit(enemy_type_a_image, self.rect.topleft)

class EnemyTypeB(Enemy):
    def draw(self, surface):
        surface.blit(enemy_type_b_image, self.rect.topleft)

class EnemyTypeC(Enemy):
    def draw(self, surface):
        surface.blit(enemy_type_c_image, self.rect.topleft)

# Item class (thrown by enemy)
class Item:
    def __init__(self, x):
        self.rect = pygame.Rect(x, 50, ITEM_WIDTH, ITEM_HEIGHT)
        self.speed = ENEMY_ITEM_SPEED

    def move(self):
        self.rect.y += self.speed

    def draw(self, surface):
        pygame.draw.rect(surface, (0, 255, 0), self.rect)  # Keep item as rectangle for now

# Bullet class (player)
class Bullet:
    def __init__(self, x, y):
        self.rect = pygame.Rect(x, y, BULLET_WIDTH, BULLET_HEIGHT)

    def move(self):
        self.rect.y -= BULLET_SPEED

    def draw(self, surface):
        pygame.draw.rect(surface, (255, 255, 255), self.rect)  # Keep bullet as rectangle for now

# Enemy Bullet class
class EnemyBullet:
    def __init__(self, x, y):
        self.rect = pygame.Rect(x, y, BULLET_WIDTH, BULLET_HEIGHT)

    def move(self):
        self.rect.y += ENEMY_BULLET_SPEED

    def draw(self, surface):
        pygame.draw.rect(surface, (0, 255, 0), self.rect)  # Keep enemy bullet as rectangle for now

# Game loop
def main():
    clock = pygame.time.Clock()
    player = Player()
    enemies = []
    items = []
    bullets = []
    enemy_bullets = []
    score = 0
    game_over = False
    level = 1  # Starting level

    enemy_spawn_timer = 0  # Timer for enemy spawn
    enemy_shoot_timer = 0   # Timer for enemy shooting

    can_shoot = True  # Variable to track if the player can shoot

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN and game_over:
                # Restart game on pressing any key after game over
                player = Player()
                enemies.clear()
                items.clear()
                bullets.clear()
                enemy_bullets.clear()
                score = 0
                level = 1  # Reset level on restart
                game_over = False
                pygame.mixer.music.play(-1)  # Restart background music

            # Allow shooting only once per key press
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and can_shoot and not game_over:
                    bullets.append(Bullet(player.rect.centerx + PLAYER_WIDTH // 2 - BULLET_WIDTH // 2, player.rect.top))
                    can_shoot = False  # Prevent further shooting until key is released

            # Reset can_shoot when the key is released
            if event.type == pygame.KEYUP:
                if event.key == pygame.K_UP:
                    can_shoot = True  # Allow shooting again when the key is released

        if not game_over:
            keys = pygame.key.get_pressed()
            if keys[pygame.K_LEFT]:
                player.move(-SPEED)
            if keys[pygame.K_RIGHT]:
                player.move(SPEED)

            # Spawn enemies at intervals
            enemy_spawn_timer += 1
            if enemy_spawn_timer > ENEMY_SPAWN_INTERVAL:  # 2 seconds
                enemy_type = random.choice([EnemyTypeA, EnemyTypeB, EnemyTypeC])  # Randomly choose an enemy type
                enemies.append(enemy_type())  # Instantiate the chosen enemy type
                enemy_spawn_timer = 0

            # Enemy shooting at intervals
            enemy_shoot_timer += 1
            if enemy_shoot_timer > ENEMY_SHOOT_INTERVAL:  # 4 seconds
                for enemy in enemies:
                    enemy_bullets.append(enemy.shoot())
                enemy_shoot_timer = 0  # Reset timer after shooting

            # Move items, bullets, and enemy bullets
            for item in items[:]:
                item.move()
                if item.rect.top > HEIGHT:
                    items.remove(item)

            for bullet in bullets[:]:
                bullet.move()
                if bullet.rect.bottom < 0:
                    bullets.remove(bullet)

            for enemy_bullet in enemy_bullets[:]:
                enemy_bullet.move()
                if enemy_bullet.rect.top > HEIGHT:
                    enemy_bullets.remove(enemy_bullet)

            # Check for scoring and level up
            if score >= level * 10:  # Level up every 10 points
                level += 1
                global ENEMY_BULLET_SPEED
                ENEMY_BULLET_SPEED += 1  # Increase enemy bullet speed by 1
                print(f"Level Up! You are now on Level {level}")

            # Winning condition: If player reaches level 8, they win
            if level >= 8 and not game_over:
                victory_sound.play()  # Play victory sound
                font = pygame.font.Font(None, 74)
                win_text = font.render('YOU WIN!', True, (0, 255, 0))
                screen.blit(win_text, (WIDTH // 2 - win_text.get_width() // 2, HEIGHT // 2 - 50))
                pygame.display.flip()
                pygame.time.delay(2000)  # Show the win message for 2 seconds
                game_over = True  # End the game after winning

            # Collision detection
            for item in items[:]:
                if item.rect.colliderect(player.rect):
                    game_over = True  # Set game over state
                    pygame.mixer.music.stop()  # Stop background music
                    game_over_sound.play()  # Play game over sound

            for bullet in bullets[:]:
                for enemy in enemies[:]:
                    if bullet.rect.colliderect(enemy.rect):
                        score += 1
                        enemy_death_sound.play()  # Play enemy death sound
                        bullets.remove(bullet)
                        enemies.remove(enemy)  # Remove the enemy that was hit
                        break  # Only process one enemy hit per bullet

            for enemy_bullet in enemy_bullets[:]:
                if enemy_bullet.rect.colliderect(player.rect):
                    game_over = True  # Set game over state
                    pygame.mixer.music.stop()  # Stop background music
                    game_over_sound.play()  # Play game over sound

            # Clear the screen
            screen.blit(background_image, (0, 0))  # Draw the background image

            # Draw everything
            player.draw(screen)
            for enemy in enemies:
                enemy.draw(screen)
            for item in items:
                item.draw(screen)
            for bullet in bullets:
                bullet.draw(screen)
            for enemy_bullet in enemy_bullets:
                enemy_bullet.draw(screen)

            # Draw score and level
            font = pygame.font.Font(None, 36)
            score_text = font.render(f'Score: {score}', True, (255, 255, 255))
            level_text = font.render(f'Level: {level}', True, (255, 255, 255))
            screen.blit(score_text, (10, 10))
            screen.blit(level_text, (10, 50))

        else:
            # Draw game over screen
            font = pygame.font.Font(None, 74)
            game_over_text = font.render('Game Over', True, (255, 0, 0))
            play_again_text = font.render('Press Any Key to Play Again', True, (0, 0, 255))
            game_over_rect = game_over_text.get_rect(center=(WIDTH // 2, HEIGHT // 2 - 20))
            play_again_rect = play_again_text.get_rect(center=(WIDTH // 2, HEIGHT // 2 + 20))
            screen.blit(game_over_text, game_over_rect)
            screen.blit(play_again_text, play_again_rect)

        pygame.display.flip()
        clock.tick(FPS)

if __name__ == "__main__":
    main()
