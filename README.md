import pygame
import random

# 初始化 Pygame
pygame.init()

# 游戏窗口设置（640x480，与题目场地一致）
width, height = 640, 480
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("贪吃蛇游戏")

# 颜色定义
BLACK = (0, 0, 0)    # 背景色
GREEN = (0, 255, 0)  # 蛇的颜色
RED = (255, 0, 0)    # 苹果的颜色

# 蛇的初始化（示例数据与题目一致）
snake = [(180, 140), (200, 140), (220, 140), (240, 140), (260, 140), (260, 160), (260, 180)]
direction = (-1, 0)  # 初始向左移动（与题目 LEFT=(-1,0) 一致）

# 苹果初始化（示例数据与题目一致）
apple = (40, 100)

clock = pygame.time.Clock()  # 控制帧率
running = True

while running:
    # 处理事件（如关闭窗口、键盘输入）
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            # 防止直接反向移动（如左移时不能直接右移）
            if event.key == pygame.K_LEFT and direction != (1, 0):
                direction = (-1, 0)
            elif event.key == pygame.K_RIGHT and direction != (-1, 0):
                direction = (1, 0)
            elif event.key == pygame.K_UP and direction != (0, 1):
                direction = (0, -1)
            elif event.key == pygame.K_DOWN and direction != (0, -1):
                direction = (0, 1)

    # 计算新蛇头位置
    head_x, head_y = snake[0]
    new_head = (head_x + direction[0] * 20, head_y + direction[1] * 20)
    snake.insert(0, new_head)

    # 检查是否吃到苹果
    if new_head == apple:
        # 重新生成苹果位置（随机在网格内）
        apple = (random.randint(0, (width // 20 - 1)) * 20, random.randint(0, (height // 20 - 1)) * 20)
    else:
        # 未吃到苹果，移除蛇尾
        snake.pop()

    # 碰撞检测（撞墙或撞自己）
    if (new_head[0] < 0 or new_head[0] >= width or new_head[1] < 0 or new_head[1] >= height) \
            or (new_head in snake[1:]):
        running = False

    # 绘制画面
    screen.fill(BLACK)  # 清屏
    for segment in snake:
        pygame.draw.rect(screen, GREEN, (segment[0], segment[1], 20, 20))  # 画蛇
    pygame.draw.rect(screen, RED, (apple[0], apple[1], 20, 20))  # 画苹果
    pygame.display.flip()  # 更新显示
    clock.tick(10)  # 控制帧率为 10 帧/秒

pygame.quit()  # 退出 Pygame
