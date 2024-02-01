import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up the game window
width, height = 460, 307
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Reckoning at Harpers Ferry")

# Function to load and scale images while preserving aspect ratio
def load_and_scale_image(file_path, target_size):
    original_image = pygame.image.load(file_path)
    original_rect = original_image.get_rect()

    # Calculate the aspect ratio
    aspect_ratio = original_rect.width / original_rect.height

    # Scale the image based on the target width, maintaining the aspect ratio
    scaled_width = target_size[0]
    scaled_height = int(scaled_width / aspect_ratio)

    return pygame.transform.scale(original_image, (scaled_width, scaled_height))

# Load images
background = pygame.image.load("background.png")
player_walking_images = [
    load_and_scale_image("character_walk1.png", (50, 50)),
    load_and_scale_image("character_walk2.png", (50, 50)),
    # Add more walking frames as needed
]
player_normal_image = load_and_scale_image("character.png", (50, 50))
raid_image = pygame.image.load("raid.png")

# Player coordinates
player_x, player_y = 10, 140

# Cutscene text
font = pygame.font.Font(None, 22)
cutscene_text = [
    "In the year 1859, ",
    "you are an abolitionist determined to end slavery.",
    "Your mission is to incite a slave rebellion at Harpers Ferry.",
    "Navigate through obstacles,",
    "avoid detection,",  
    "and inspire the oppressed.",
    "Press SPACE to begin your mission.",
]

# Objective text
objective_text = "Objective: Find John Brown and join the rebellion"

class NPC(pygame.sprite.Sprite):
    def __init__(self, idle_images, position):
        super().__init__()
        self.idle_images = [load_and_scale_image(image_path, (50, 50)) for image_path in idle_images]
        self.image_index = 0
        self.image = self.idle_images[self.image_index]
        self.rect = self.image.get_rect(topleft=position)
        self.idle_timer = 0
        self.idle_time_threshold = 15 # Adjust as needed

    def update(self):
        self.idle_timer += 1

        # Switch between idle images based on the timer
        if self.idle_timer >= self.idle_time_threshold:
            self.image_index = (self.image_index + 1) % len(self.idle_images)
            self.image = self.idle_images[self.image_index]
            self.idle_timer = 0

# Instantiate the NPC with two idle images
npc = NPC(["npcidle1.png", "npcidle2.png"], (200, 140))

# Function to render text
def render_text(text, font, color, position):
    rendered_text = font.render(text, True, color)
    screen.blit(rendered_text, position)

# Function to render player sprite
def render_player_sprite():
    global player_y # Make sure to use the global variable

    if keys[pygame.K_LEFT] or keys[pygame.K_RIGHT]:
        # If moving left or right, alternate between walking frames
        walking_frame = (frame // 3) % len(player_walking_images)
        walking_image = player_walking_images[walking_frame]

        # Adjust the player_y coordinate to raise the walking animation
        raised_player_y = player_y - 27 # Experiment with different values
        screen.blit(walking_image, (player_x, raised_player_y))
    else:
        # If not moving, display the normal sprite
        screen.blit(player_normal_image, (player_x, player_y))

# Cutscene loop
cutscene_active = True
cutscene_frame = 0
while cutscene_active:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    if keys[pygame.K_SPACE]:
        cutscene_active = False

    # Rendering cutscene text with a background box
    screen.blit(pygame.transform.scale(background, (width, height)), (0, 0))
    pygame.draw.rect(screen, (0, 0, 0, 150), (10, 10, width - 20, height - 20)) # Background box
    for i, line in enumerate(cutscene_text):
        render_text(line, font, (255, 255, 255), (20, 20 + i * 40))

    # Increment frame count
    cutscene_frame += 1

    # Hide text after 300 frames (adjust as needed)
    if cutscene_frame > 1000:
        cutscene_active = False

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    pygame.time.Clock().tick(30)

# Function to calculate distance between two points
def calculate_distance(point1, point2):
    return ((point1[0] - point2[0])**2 + (point1[1] - point2[1])**2)**0.5

# Dialogue state and options
dialogue_active = False
dialogue_window_active = False
npc_dialogue = [
    ("Hello there! My name is John Brown. Why do you fight against slavery?", ["Freedom for all!", "I have my reasons.", "I support slavery"]),
    ("Would you like to join the rebellion?", ["Yes", "No"]),
    # Add more dialogue and answer choices as needed
]
selected_answer = 0

# Dialogue window dimensions
dialogue_width, dialogue_height = 400, 200

def display_dialogue_window():
  smaller_font = pygame.font.Font(None, 14)
  pygame.draw.rect(screen, (0, 0, 0, 150), (width // 2 - dialogue_width // 2, height // 2 - dialogue_height // 2, dialogue_width, dialogue_height)) # Background box
  render_text(npc_dialogue[0][0], smaller_font, (255, 255, 255), (width // 2 - dialogue_width // 2 + 20, height // 2 - dialogue_height // 2 + 20))

  # Display answer choices
  for i, choice in enumerate(npc_dialogue[0][1]):
      color = (255, 255, 255)
      if i == selected_answer:
          color = (255, 0, 0) # Highlight the selected answer
      render_text(choice, font, color, (width // 2 - dialogue_width // 2 + 20, height // 2 - dialogue_height // 2 + 80 + i * 30))

# Main game loop
frame = 0
key_pressed = False  # Flag to track whether a key is pressed
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if not dialogue_active:
                key_pressed = True
                if event.key == pygame.K_LEFT:
                    player_x -= 5
                elif event.key == pygame.K_RIGHT:
                    player_x += 5
                elif event.key == pygame.K_RETURN:
                    # Enter dialogue mode and open the dialogue window
                    dialogue_active = True
                    selected_answer = 0
                    dialogue_window_active = True
        elif event.type == pygame.KEYUP:
            key_pressed = False

    keys = pygame.key.get_pressed()

    # Handle user input
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if not dialogue_active:
                if event.key == pygame.K_LEFT:
                    player_x -= 5
                elif event.key == pygame.K_RIGHT:
                    player_x += 5
                elif event.key == pygame.K_RETURN:
                    # Enter dialogue mode and open the dialogue window
                    dialogue_active = True
                    selected_answer = 0
                    dialogue_window_active = True
            elif event.key == pygame.K_DOWN:
                selected_answer = (selected_answer + 1) % len(npc_dialogue[0][1])
            elif event.key == pygame.K_UP:
                selected_answer = (selected_answer - 1) % len(npc_dialogue[0][1])
  
    keys = pygame.key.get_pressed()
  
    # Reset the flag when no keys are pressed
    key_pressed = any(keys)
  
    # Handle user input
    if not dialogue_active:
        if keys[pygame.K_LEFT]:
            player_x -= 5
        if keys[pygame.K_RIGHT]:
            player_x += 5
        if keys[pygame.K_RETURN]:
            # Enter dialogue mode and open the dialogue window
            dialogue_active = True
            selected_answer = 0
            dialogue_window_active = True
  
    # Update NPC
    npc.update()
   
    # Calculate distance between player and NPC
    distance_to_npc = calculate_distance((player_x, player_y), npc.rect.topleft)
  
    # Handle NPC dialogue
    if dialogue_active:
        # Process user's choice during dialogue
        if key_pressed and keys[pygame.K_SPACE]:
            if npc_dialogue[0][1][selected_answer] == "Freedom for all!":
  
                # Close the dialogue window and update the game state
                dialogue_active = False
                dialogue_window_active = False

                #remove npc
                npc.rect.topleft = (-100, -100)

                game_active = False

                break
  
                # Change the background to raid.png
                background = pygame.image.load("raid.png")
                background = pygame.transform.scale(background, (width, height))
  
            elif npc_dialogue[0][1][selected_answer] == "I have my reasons.":
                # Handle other choices if needed
                pass
            elif npc_dialogue[0][1][selected_answer] == "I support slavery":
                # Handle other choices if needed
                pass
  
            # End the dialogue
            dialogue_active = False
            dialogue_window_active = False
    
    # Rendering
    screen.blit(pygame.transform.scale(background, (width, height)), (0, 0))
    render_player_sprite()
    
    # Render NPC
    screen.blit(npc.image, npc.rect.topleft)
    
    # Display the objective overlay with a stylish background box
    objective_font = pygame.font.Font(None, 24)
    objective_rendered_text = objective_font.render(objective_text, True, (255, 255, 255))
    objective_background_rect = pygame.Rect(20, 20, objective_rendered_text.get_width() + 20, objective_rendered_text.get_height() + 10)
    pygame.draw.rect(screen, (0, 0, 0, 150), objective_background_rect)  # Stylish background box
    screen.blit(objective_rendered_text, (30, 25))
    
      # Display interaction alert if close to the NPC
    if dialogue_active:
          # Handle answer selection during dialogue
          if key_pressed:
              if keys[pygame.K_SPACE]:
                  # Process player's choice during dialogue
                  dialogue_active = False
                  dialogue_window_active = False
                  print("Selected Answer:", npc_dialogue[0][1][selected_answer])
              elif keys[pygame.K_DOWN]:
                  # Toggle between dialogue options with each key press
                if npc_dialogue and npc_dialogue[0] and npc_dialogue[0][1]:
                  selected_answer = (selected_answer + 1) % len(npc_dialogue[0][1])

              elif keys[pygame.K_UP]:
                if npc_dialogue and npc_dialogue[0] and npc_dialogue[0][1]:
                  selected_answer = (selected_answer - 1) % len(npc_dialogue[0][1])

    
          # Display the dialogue and answer choices
          if dialogue_window_active:
              display_dialogue_window()
    
    elif distance_to_npc < 50:  # Adjust the distance threshold as needed
          alert_font = pygame.font.Font(None, 24)
          alert_text = alert_font.render("Press ENTER to interact", True, (255, 255, 255))
          alert_background_rect = pygame.Rect(20, height - 60, alert_text.get_width() + 20, alert_text.get_height() + 10)
          pygame.draw.rect(screen, (0, 0, 0, 150), alert_background_rect)  # Stylish background box
          screen.blit(alert_text, (30, height - 55))
          if key_pressed and keys[pygame.K_RETURN]:
              # Enter dialogue mode
              dialogue_active = True
              selected_answer = 0
              dialogue_window_active = True
    else:
          dialogue_active = False
    
      # Display the dialogue and answer choices
    if dialogue_window_active:
          display_dialogue_window()
    
      # Update the display
    pygame.display.flip()
    
      # Increment frame count
    frame += 1
    
      # Cap the frame rate
    pygame.time.Clock().tick(30)
    
          
