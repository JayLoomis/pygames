# Pygame Player Input
Input is extremely important in game development. Without responsive input from
the user, you don't have a game at all. This is what I've learned about player
input in pygame.

## The dangerous allure of direct input
You can directly query the state of both the keyboard and mouse through pygame.

For the keyboard, it looks something like this.

``` python
# Check for pressed wasd keys for movement.
keys = pygame.key.get_pressed()
if keys[pygame.K_w]:
    player_pos.y -= y_velocity
if keys[pygame.K_s]:
    player_pos.y += y_velocity
if keys[pygame.K_a]:
    player_pos.x -= x_velocity
if keys[pygame.K_d]:
    player_pos.x += x_velocity
```

And for the mouse, something like this.

``` python
# Move toward the mouse pointer (orthogonally) when the mouse button is down.
if python.mouse.get_pressed()[0]:
    mouse_x = pygame.mouse.get_pos()[0]
    mouse_y = pygame.mouse.get_pos()[1]

    delta_x = mouse_x - player_pos.x
    delta_y = mouse_y - player_pos.y

    if delta_x > delta_y:
        if delta_x < 0:
            player.pos.x -= x_velocity
        else:
            player.pos.x += x_velocity
    else:
        if delta_y < 0:
            player.pos.y -= y_velocity
        else:
            player.pos.y += y_velocity
# So, actually, this code is incomplete! It doesn't handle the case where the
#  mouse deltas are identical. And it doesn't account for the width of the
#  character sprite. But the basic procedure as it applies to the mouse class
#  should be good--though it is untested, so there's that!
```

In both cases, there's a problem to consider. What do you want your game to do
when the player holds down a key or button? You need to build logic into your
code to handle that, or else you'll end up zipping around the screen every cycle.
Even if you work that out, there's a more fundamental problem.

Pygame is designed to run on an event model. The correct way to handle inputs is
by responding to their associated events.


<!--
         10        20        30        40        50        60        70        80
----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
-->
