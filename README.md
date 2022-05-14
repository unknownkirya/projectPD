# projectPD

    #импортируем необходимые библиотеки

    import pygame
    import time
    import random

    #вызываем функцию для подготовки модулей pygame к дальнейшей работе 

    pygame.init()

    #описываем цвета

    white = (255, 255, 255)
    yellow = (255, 255, 102)
    orange = (255, 136, 0)
    red = (213, 50, 80)
    green = (255, 255, 120)
    fuchsia = (255, 0, 255)
    lime = (135, 240, 10)
    black = (0, 0, 0)

    #задаём размеры дисплея, на котором будет происходить игровой процесс

    dis_width = 660
    dis_height = 440

    #создаём дисплей и задаём название игры 

    dis = pygame.display.set_mode((dis_width, dis_height))
    pygame.display.set_caption('Змейка')

    #начинаем отсчёт времени 

    clock = pygame.time.Clock()

    #задаём размеры самой змейки и её скорость  

    snake_block = 10
    snake_speed = 11

    #задаём размеры сопровождающих игрока надписей

    font_style = pygame.font.SysFont("bahnschrift", 25)
    score_font = pygame.font.SysFont("comicsansms", 35)

    #функция для контроля игрового счёта (съеденных фрагментов еды) 

    def Your_score(score):
        value = score_font.render("Your Score: " + str(score), True, fuchsia)
        dis.blit(value, [0, 0])

    #функция для отрисовки самой змейки 

    def snake_pct(snake_block, snake_list):
        for x in snake_list:
            pygame.draw.rect(dis, black, [x[0], x[1], snake_block, snake_block])

    #функция для создания сообщений внутри игры 

    def message(msg, color):
        mesg = font_style.render(msg, True, color)
        dis.blit(mesg, [dis_width / 6, dis_height / 3])

    #функция, в которой и происходит игровой процесс 

    def game_process():
        global snake_speed
        game_over = False
        game_close = False

    #задаём начальные координаты змейки
    
    x1 = dis_width / 2
    y1 = dis_height / 2

    #задаём начальное изменение координат
    
    x1_change = 0
    y1_change = 0

    snake_List = []
    Length_of_snake = 1

    #координаты еды
    
    foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
 
    while not game_over:
    
        #описываем условие проигрыша
        
        while game_close == True:
            dis.fill(lime)
            message("You Lost! Press C-Play Again or Q-Quit", red)
            Your_score(Length_of_snake - 1)
            pygame.display.update()
 
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                
                    #выключаем игру
                    
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                        
                    #продолжаем играть
                    
                    if event.key == pygame.K_c:
                        dis.fill(lime)
                        #пытаемся добавить уровни сложности в игру
                        '''
                        message("Select the difficulty level: E-easy, M-middle, H-hard", red)
                        pygame.display.update()
                        for event in pygame.event.get():
                            if event.type == pygame.KEYUP:
                                if event.key == pygame.K_e:
                                    snake_speed = 7
                                if event.key == pygame.K_m:
                                    snake_speed = 10
                                if event.key == pygame.K_h:
                                    snake_speed = 15
                                pygame.display.update()
                        clock.tick(snake_speed)
                        '''
                                
                        #заново запускаем игровой процесс          
                        
                        game_process()

        #прописываем игровые события 
        
        for event in pygame.event.get():
        
            #условие выхода из игры
            
            if event.type == pygame.QUIT:
                game_over = True
                
            #условия передвижения змейки
            
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0
        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True
            
        #задаём координаты змейки после нажатия на одну из стрелок
        
        x1 += x1_change
        y1 += y1_change
        dis.fill(lime)
        
        #отрисовываем светлячков
        
        #меняется цвет тела
        
        n = random.randint(1, 2)
        if n == 1:
            pygame.draw.rect(dis, orange, [foodx, foody, snake_block, snake_block])
            pygame.draw.circle(dis, green, (foodx + 2, foody + 2), snake_block / 2, 0)
        else:
            pygame.draw.rect(dis, red, [foodx, foody, snake_block, snake_block])
            pygame.draw.circle(dis, yellow, (foodx + 2, foody + 2), snake_block / 2, 0)
        n2 = random.randint(1, 2)
        
        #меняется положение усиков
        
        if n2 == 1:
            pygame.draw.line(dis, black, (foodx, foody - 3), (foodx - 5, foody - 7), 1)
            pygame.draw.line(dis, black, (foodx + 3, foody - 3), (foodx + 8, foody - 7), 1)
        else:
            pygame.draw.line(dis, black, (foodx, foody - 3), (foodx, foody - 7), 1)
            pygame.draw.line(dis, black, (foodx + 3, foody - 3), (foodx + 3, foody - 7), 1)
            
        #лапки
        
        pygame.draw.circle(dis, black, (foodx + 9, foody + 10), snake_block / 7, 0)
        pygame.draw.circle(dis, black, (foodx + 4, foody + 10), snake_block / 7, 0)
        pygame.draw.circle(dis, black, (foodx, foody + 10), snake_block / 7, 0)
        
        #рот
        
        n3 = random.randint(1, 2)
        if n3 == 1:
            pygame.draw.circle(dis, black, (foodx + 2, foody + 4), snake_block / 5, 1)
        else:
            pygame.draw.circle(dis, black, (foodx + 2, foody + 4), snake_block / 7, 0)
            
        #глаза
        
        pygame.draw.circle(dis, black, (foodx, foody), snake_block / 7, 0)
        pygame.draw.circle(dis, black, (foodx + 4, foody), snake_block / 7, 0)
        
        #описываем координаты головы змейки
        
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        
        #организуем корректное изображение змейки при движении
        
        if len(snake_List) > Length_of_snake:
            del snake_List[0]
            
        #повернули против движения змейки при счёте >=2
        
        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True
        
        snake_pct(snake_block, snake_List)
        Your_score(Length_of_snake - 1)
        
        #обновляем дисплей
        
        pygame.display.update()

        #змейка съела еду
        
        if x1 == foodx and y1 == foody:
            foodx = int(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            foody = int(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1
 
        clock.tick(snake_speed)
    
    pygame.quit()
    quit()
 
 
    game_process()
