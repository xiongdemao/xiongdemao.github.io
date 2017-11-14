---
layout: '[poto]'
title: 是AI就躲个飞机-纯Python实现人工智能
date: 2017-11-13 21:06:58
tags: [机器学习,笔记]
categories: [机器学习]
---
转自：[是AI就躲个飞机-纯Python实现人工智能](http://blog.csdn.net/u014365862/article/details/54380422)


很久以前微信流行过一个小游戏：打飞机，这个游戏简单又无聊。在2017年来临之际，我就实现一个超级弱智的人工智能（AI），这货可以躲避从屏幕上方飞来的飞机。本帖只使用纯Python实现，不依赖任何高级库。

本文的AI基于[neuro-evolution](https://en.wikipedia.org/wiki/Neuroevolution)，首先简单科普一下neuro-evolution。从neuro-evolution这个名字就可以看出它由两部分组成-neuro and evolution，它是使用进化算法（遗传算法是进化算法的一种）提升人工神经网络的机器学习技术，其实就是用进化算法改进并选出最优的神经网络。

   [使用TPOT自动选择scikit-learn机器学习模型和参数](http://blog.topspeedsnail.com/archives/10709)

## neuro-evolution

定义一些变量：

```[python] view plain copy

    import math  
    import random  
       
    # 神经网络3层, 1个隐藏层; 4个input和1个output  
    network = [4, [16], 1]  
    # 遗传算法相关  
    population = 50  
    elitism = 0.2   
    random_behaviour = 0.1  
    mutation_rate = 0.5  
    mutation_range = 2  
    historic = 0  
    low_historic = False  
    score_sort = -1  
    n_child = 1  
```
定义神经网络：

```[python] view plain copy

    # 激活函数  
    def sigmoid(z):  
        return 1.0/(1.0+math.exp(-z))  
    # random number  
    def random_clamped():  
        return random.random()*2-1  
       
    # "神经元"  
    class Neuron():  
        def __init__(self):  
            self.biase = 0  
            self.weights = []  
       
        def init_weights(self, n):  
            self.weights = []  
            for i in range(n):  
                self.weights.append(random_clamped())  
        def __repr__(self):  
            return 'Neuron weight size:{}  biase value:{}'.format(len(self.weights), self.biase)  
       
    # 层  
    class Layer():  
        def __init__(self, index):  
            self.index = index  
            self.neurons = []  
       
        def init_neurons(self, n_neuron, n_input):  
            self.neurons = []  
            for i in range(n_neuron):  
                neuron = Neuron()  
                neuron.init_weights(n_input)  
                self.neurons.append(neuron)  
       
        def __repr__(self):  
            return 'Layer ID:{}  Layer neuron size:{}'.format(self.index, len(self.neurons))  
       
    # 神经网络  
    class NeuroNetwork():  
        def __init__(self):  
            self.layers = []  
       
        # input:输入层神经元数 hiddens:隐藏层 output:输出层神经元数  
        def init_neuro_network(self, input, hiddens , output):  
            index = 0  
            previous_neurons = 0  
            # input  
            layer = Layer(index)  
            layer.init_neurons(input, previous_neurons)  
            previous_neurons = input  
            self.layers.append(layer)  
            index += 1  
            # hiddens  
            for i in range(len(hiddens)):  
                layer = Layer(index)  
                layer.init_neurons(hiddens[i], previous_neurons)  
                previous_neurons = hiddens[i]  
                self.layers.append(layer)  
                index += 1  
            # output  
            layer = Layer(index)  
            layer.init_neurons(output, previous_neurons)  
            self.layers.append(layer)  
       
        def get_weights(self):  
            data = { 'network':[], 'weights':[] }  
            for layer in self.layers:  
                data['network'].append(len(layer.neurons))  
                for neuron in layer.neurons:  
                    for weight in neuron.weights:  
                        data['weights'].append(weight)  
            return data  
       
        def set_weights(self, data):  
            previous_neurons = 0  
            index = 0  
            index_weights = 0  
       
            self.layers = []  
            for i in data['network']:  
                layer = Layer(index)  
                layer.init_neurons(i, previous_neurons)  
                for j in range(len(layer.neurons)):  
                    for k in range(len(layer.neurons[j].weights)):  
                        layer.neurons[j].weights[k] = data['weights'][index_weights]  
                        index_weights += 1  
                previous_neurons = i  
                index += 1  
                self.layers.append(layer)  
       
        # 输入游戏环境中的一些条件(如敌机位置), 返回要执行的操作  
        def feed_forward(self, inputs):  
            for i in range(len(inputs)):  
                self.layers[0].neurons[i].biase = inputs[i]  
       
            prev_layer = self.layers[0]  
            for i in range(len(self.layers)):  
                # 第一层没有weights  
                if i == 0:  
                    continue  
                for j in range(len(self.layers[i].neurons)):  
                    sum = 0  
                    for k in range(len(prev_layer.neurons)):  
                        sum += prev_layer.neurons[k].biase * self.layers[i].neurons[j].weights[k]  
                    self.layers[i].neurons[j].biase = sigmoid(sum)  
                prev_layer = self.layers[i]  
       
            out = []  
            last_layer = self.layers[-1]  
            for i in range(len(last_layer.neurons)):  
                out.append(last_layer.neurons[i].biase)  
            return out  
       
        def print_info(self):  
            for layer in self.layers:  
                print(layer)  
```
遗传算法：

```[python] view plain copy

    # "基因组"  
    class Genome():  
        def __init__(self, score, network_weights):  
            self.score = score  
            self.network_weights = network_weights  
       
    class Generation():  
        def __init__(self):  
            self.genomes = []  
       
        def add_genome(self, genome):  
            i = 0  
            for i in range(len(self.genomes)):  
                if score_sort < 0:  
                    if genome.score > self.genomes[i].score:  
                        break  
                else:  
                    if genome.score < self.genomes[i].score:  
                        break  
            self.genomes.insert(i, genome)  
       
            # 杂交+突变  
        def breed(self, genome1, genome2, n_child):  
            datas = []  
            for n in range(n_child):  
                data = genome1  
                for i in range(len(genome2.network_weights['weights'])):  
                    if random.random() <= 0.5:  
                        data.network_weights['weights'][i] = genome2.network_weights['weights'][i]  
       
                for i in range(len(data.network_weights['weights'])):  
                    if random.random() <= mutation_rate:  
                        data.network_weights['weights'][i] += random.random() * mutation_range * 2 - mutation_range  
                datas.append(data)  
            return datas  
       
            # 生成下一代  
        def generate_next_generation(self):  
            nexts = []  
            for i in range(round(elitism*population)):  
                if len(nexts) < population:  
                    nexts.append(self.genomes[i].network_weights)  
       
            for i in range(round(random_behaviour*population)):  
                n = self.genomes[0].network_weights  
                for k in range(len(n['weights'])):  
                    n['weights'][k] = random_clamped()  
                if len(nexts) < population:  
                    nexts.append(n)  
       
            max_n = 0  
            while True:  
                for i in range(max_n):  
                    childs = self.breed(self.genomes[i], self.genomes[max_n], n_child if n_child > 0 else 1)  
                    for c in range(len(childs)):  
                        nexts.append(childs[c].network_weights)  
                        if len(nexts) >= population:  
                            return nexts  
                max_n += 1  
                if max_n >= len(self.genomes)-1:  
                    max_n = 0  

```
NeuroEvolution：

```[python] view plain copy

    class Generations():  
        def __init__(self):  
            self.generations = []  
       
        def first_generation(self):  
            out = []  
            for i in range(population):  
                nn = NeuroNetwork()  
                nn.init_neuro_network(network[0], network[1], network[2])  
                out.append(nn.get_weights())  
            self.generations.append(Generation())  
            return out  
              
        def next_generation(self):  
            if len(self.generations) == 0:  
                return False  
       
            gen = self.generations[-1].generate_next_generation()  
            self.generations.append(Generation())  
            return gen  
       
        def add_genome(self, genome):  
            if len(self.generations) == 0:  
                return False  
       
            return self.generations[-1].add_genome(genome)  
       
    class NeuroEvolution():  
        def __init__(self):  
            self.generations = Generations()  
       
        def restart(self):  
            self.generations = Generations()  
       
        def next_generation(self):  
            networks = []  
            if len(self.generations.generations) == 0:  
                networks = self.generations.first_generation()  
            else:  
                networks = self.generations.next_generation()  
       
            nn = []  
            for i in range(len(networks)):  
                n = NeuroNetwork()  
                n.set_weights(networks[i])  
                nn.append(n)  
       
            if low_historic:  
                if len(self.generations.generations) >= 2:  
                    genomes = self.generations.generations[len(self.generations.generations) - 2].genomes  
                    for i in range(genomes):  
                        genomes[i].network = None  
       
            if historic != -1:  
                if len(self.generations.generations) > historic+1:  
                    del self.generations.generations[0:len(self.generations.generations)-(historic+1)]  
       
            return nn  
       
        def network_score(self, score, network):  
            self.generations.add_genome(Genome(score, network.get_weights()))  
```
是AI就躲个飞机
```[python] view plain copy

    import pygame  
    import sys  
    from pygame.locals import *  
    import random  
    import math  
       
    import neuro_evolution  
       
    BACKGROUND = (200, 200, 200)  
    SCREEN_SIZE = (320, 480)  
       
    class Plane():  
        def __init__(self, plane_image):  
            self.plane_image = plane_image  
            self.rect = plane_image.get_rect()  
       
            self.width = self.rect[2]  
            self.height = self.rect[3]  
            self.x = SCREEN_SIZE[0]/2 - self.width/2  
            self.y = SCREEN_SIZE[1] - self.height  
       
            self.move_x = 0  
            self.speed = 2  
       
            self.alive = True  
       
        def update(self):  
            self.x += self.move_x * self.speed  
       
        def draw(self, screen):  
            screen.blit(self.plane_image, (self.x, self.y, self.width, self.height))  
       
        def is_dead(self, enemes):  
            if self.x < -self.width or self.x + self.width > SCREEN_SIZE[0]+self.width:  
                return True  
       
            for eneme in enemes:  
                if self.collision(eneme):  
                    return True  
            return False  
       
        def collision(self, eneme):  
            if not (self.x > eneme.x + eneme.width or self.x + self.width < eneme.x or self.y > eneme.y + eneme.height or self.y + self.height < eneme.y):  
                return True  
            else:  
                return False  
       
        def get_inputs_values(self, enemes, input_size=4):  
            inputs = []  
       
            for i in range(input_size):  
                inputs.append(0.0)  
       
            inputs[0] = (self.x*1.0 / SCREEN_SIZE[0])  
            index = 1  
            for eneme in enemes:  
                inputs[index] = eneme.x*1.0 / SCREEN_SIZE[0]  
                index += 1  
                inputs[index] = eneme.y*1.0 / SCREEN_SIZE[1]  
                index += 1  
            #if len(enemes) > 0:  
                #distance = math.sqrt(math.pow(enemes[0].x + enemes[0].width/2 - self.x + self.width/2, 2) + math.pow(enemes[0].y + enemes[0].height/2 - self.y + self.height/2, 2));  
            if len(enemes) > 0 and self.x < enemes[0].x:  
                inputs[index] = -1.0  
                index += 1  
            else:  
                inputs[index] = 1.0  
       
            return inputs  
       
    class Enemy():  
        def __init__(self, enemy_image):  
            self.enemy_image = enemy_image  
            self.rect = enemy_image.get_rect()  
       
            self.width = self.rect[2]  
            self.height = self.rect[3]  
            self.x = random.choice(range(0, int(SCREEN_SIZE[0] - self.width/2), 71))  
            self.y = 0  
       
        def update(self):  
            self.y += 6  
       
        def draw(self, screen):  
            screen.blit(self.enemy_image, (self.x, self.y, self.width, self.height))  
       
        def is_out(self):  
            return True if self.y >= SCREEN_SIZE[1] else False  
       
    class Game():  
        def __init__(self):  
            pygame.init()  
            self.screen = pygame.display.set_mode(SCREEN_SIZE)  
            self.clock = pygame.time.Clock()  
            pygame.display.set_caption('是AI就躲个飞机')  
       
            self.ai = neuro_evolution.NeuroEvolution()  
            self.generation = 0  
       
            self.max_enemes = 1  
                    # 加载飞机、敌机图片  
            self.plane_image = pygame.image.load('plane.png').convert_alpha()  
            self.enemy_image = pygame.image.load('enemy.png').convert_alpha()  
       
        def start(self):  
            self.score = 0  
            self.planes = []  
            self.enemes = []  
       
            self.gen = self.ai.next_generation()  
            for i in range(len(self.gen)):  
                plane = Plane(self.plane_image)  
                self.planes.append(plane)  
       
            self.generation += 1  
            self.alives = len(self.planes)  
       
        def update(self, screen):  
            for i in range(len(self.planes)):  
                if self.planes[i].alive:  
                    inputs = self.planes[i].get_inputs_values(self.enemes)  
                    res = self.gen[i].feed_forward(inputs)  
                    if res[0] < 0.45:  
                        self.planes[i].move_x = -1  
                    elif res[0] > 0.55:  
                        self.planes[i].move_x = 1  
       
       
                    self.planes[i].update()  
                    self.planes[i].draw(screen)  
       
                    if self.planes[i].is_dead(self.enemes) == True:  
                        self.planes[i].alive = False  
                        self.alives -= 1  
                        self.ai.network_score(self.score, self.gen[i])  
                        if self.is_ai_all_dead():  
                            self.start()  
       
              
            self.gen_enemes()  
       
            for i in range(len(self.enemes)):  
                self.enemes[i].update()  
                self.enemes[i].draw(screen)  
                if self.enemes[i].is_out():  
                    del self.enemes[i]  
                    break  
       
            self.score += 1  
       
            print("alive:{}, generation:{}, score:{}".format(self.alives, self.generation, self.score), end='\r')  
       
        def run(self, FPS=1000):  
            while True:  
                for event in pygame.event.get():  
                    if event.type == QUIT:  
                        pygame.quit()  
                        sys.exit()  
       
                self.screen.fill(BACKGROUND)  
       
                self.update(self.screen)  
       
                pygame.display.update()  
                self.clock.tick(FPS)  
       
        def gen_enemes(self):  
            if len(self.enemes) < self.max_enemes:  
                enemy = Enemy(self.enemy_image)  
                self.enemes.append(enemy)  
       
        def is_ai_all_dead(self):  
            for plane in self.planes:  
                if plane.alive:  
                    return False  
            return True  
       
       
    game = Game()  
    game.start()  
    game.run()  
```

## AI的工作逻辑

假设你是AI，你首先繁殖一个种群（50个个体），开始的个体大都是歪瓜裂枣（上来就被敌机撞）。但是，即使是歪瓜裂枣也有表现好的，在下一代，你会使用这些表现好的再繁殖一个种群，经过代代相传，存活下来的个体会越来越优秀。其实就是仿达尔文进化论，种群->自然选择->优秀个体->杂交、变异->种群->循环n世代。

ai开始时候的表现：
![是AI就躲个飞机 - 纯Python实现人工智能图片被拉扁了 sorry](http://blog.topspeedsnail.com/wp-content/uploads/2016/12/ai1.gif)

经过几百代之后，ai开始娱乐的躲飞机：

![是AI就躲个飞机 - 纯Python实现人工智能](http://blog.topspeedsnail.com/wp-content/uploads/2016/12/ai2.gif)



代码：https://github.com/xiongdemao/py_game_AI_plane.git
