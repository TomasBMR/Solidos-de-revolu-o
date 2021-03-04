#Solidos de revolução
import pygame
import sys
import math
import random

try:
    pygame.init()
except:
    print("modulo pygame não inicializou")

largura = 900
altura = 600

preto = (0,0,0)
branco = (255,255,255)
vermelho = (255, 0, 0)
verde = (0, 255,0)
azul = (0, 0, 255)
amarelo = (255, 255, 0)
ciano = (0, 255, 255)
magenta = (255, 0, 255)
cinza = (220, 220, 220)
cinza_escuro = (15, 20, 40)
azul_escuro = (10, 10, 40)
azul_escuro2 = (40, 40, 160)
vermelho_escuro = (160, 50, 50)
verde_escuro = (50, 160, 50)
ciano_escuro = (0, 200, 200)
magenta_escuro = (200, 0, 200)

pi = math.pi

FONT = pygame.font.Font(None, 23)
FONT1 = pygame.font.Font(None, 36) 

tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption('Solidos de rotação')

def dist(p,q):
    return math.sqrt(sum((px - qx) ** 2.0 for px, qx in zip(p, q)))

def MultMat(a,b):
    col_a = len(a[0])
    lin_a = len(a)
    col_b = len(b[0])
    lin_b = len(b) 
    
    if col_a != lin_b:
        raise Exception("colunas de a e linhas de b devem ter o mesmo tamanho")

    c = []
    for lin in range(lin_a):
        c.append([])

    
    for i in range(lin_a):
        for j in range(col_b):
            val = 0
            for k in range(lin_b):
                val += a[i][k]*b[k][j]
            c[i].append(val)
    return c

def SomaMat(a, b):
    col_a = len(a[0])
    lin_a = len(a)
    col_b = len(b[0])
    lin_b = len(b)
    c = []
    
    if col_a != col_b or lin_a != lin_b:
        raise Exception("matrizes devem ter as mesmas proporções")

    for i in range(len(a)):
        lin = []
        for j in range(len(a[0])):
            lin.append(a[i][j]+b[i][j])
        c.append(lin)
    return c

def SomaFloat(aSomar, matriz):
    for i in range(len(matriz)):
        for j in range(len(matriz[0])):
            matriz[i][j] += aSomar
    return matriz

def MultFloat(aMultiplicar, matriz):
    for i in range(len(matriz)):
        for j in range(len(matriz[0])):
            matriz[i][j] *= aMultiplicar
    return matriz

def MatPraVec(matriz):
    if len(matriz) > 2:
        return (matriz[0][0], matriz[1][0], matriz[2][0])
    return (matriz[0][0], matriz[1][0])

def VecPraMat(vetor):
    matriz = []
    matriz.append([vetor[0]])
    matriz.append([vetor[1]])
    matriz.append([0])
    return matriz

def gerarSolido(face, numFaces):
    pontosSolido = []
    bordas = []
    faces = []
    novaFace = []
    for pnt in face:
        novaFace.append(VecPraMat(pnt))

    for i in range(numFaces):
        ang = 2*pi*i/numFaces
        rodarX = [[math.cos(ang), 0, -math.sin(ang)],
                  [0,             1,              0],
                  [math.sin(ang), 0,  math.cos(ang)]]
        for j in range(len(novaFace)):
            pontosSolido.append(MultMat(rodarX, novaFace[j]))
    for i in range(numFaces):
        for j in range(len(novaFace)):
            if j+1<len(novaFace):
                faces.append((j+i*len(novaFace),j+i*len(novaFace)+1))
            if j+len(novaFace)*(i+1)+1>len(pontosSolido):
                bordas.append((j+len(novaFace)*i,j+len(novaFace)*(i+1)-len(pontosSolido)))
            else:
                bordas.append((j+len(novaFace)*i,j+len(novaFace)*(i+1)))
        

            
    return pontosSolido, bordas, faces

class Botao:
    def __init__(self, cor_cima, cor_fora, borda_cor, x, y, w, h, texto_in=None, texto_at=None):
        self.cor_cima = cor_cima
        self.cor_fora = cor_fora
        self.cor = cor_fora
        self.borda_c = borda_cor
        self.x = x
        self.y = y
        self.w = w
        self.h = h
        self.rect = pygame.Rect(x, y, w, h)
        self.borda = pygame.Rect(x - 2, y - 2, w + 4, h + 4)
        self.ativo = False
        self.texto_in = texto_in
        self.texto_at = texto_at
    
    def evento(self, event):
        pos = pygame.mouse.get_pos()
        if pos[0] >= self.x and pos[0] <= self.x + self.w and pos[1] >= self.y and pos[1] <= self.y + self.h:
            self.cor = self.cor_cima
            if event.type == pygame.MOUSEBUTTONDOWN:
                self.ativo = not self.ativo
        else:
            self.cor = self.cor_fora

    def draw(self, fundo):
        pygame.draw.rect(fundo, self.cor, self.rect)
        if self.ativo:
            self.superficie = FONT.render(self.texto_at, True, preto)
            pygame.draw.rect(fundo, self.borda_c, self.borda, 3)
        else:
            self.superficie = FONT.render(self.texto_in, True, preto)
        fundo.blit(self.superficie, (self.x + self.w/2 - self.superficie.get_width()/2, self.y + self.h/2 - self.superficie.get_height()/2))

    def getState(self):
        return self.ativo
    
    def setState(self, state):
        self.ativo = state

class BarraInf:
    def __init__(self, nome, x, y, tamanho, larg, mini, maxi, horizontal, pos_i, cor):
            self.nome = nome
            self.x = x
            self.y = y
            self.cor = cor
            self.mini = mini
            self.maxi = maxi
            self.tamanho = tamanho
            self.larg = larg
            self.horizontal = horizontal
            self.ativo = False
            if self.horizontal:
                self.w = self.tamanho
                self.h = self.larg
                self.x_i = pos_i + self.x
                self.y_i = self.y
                self.valor = int(((self.x_i-self.x) - 0) * (self.maxi - self.mini) / (self.tamanho - self.larg - 0) + self.mini)
            elif not self.horizontal:
                self.w = self.larg
                self.h = self.tamanho
                self.x_i = self.x
                self.y_i = pos_i + self.y
                self.valor = int(((self.y_i-self.y) - 0) * (self.maxi - self.mini) / (self.tamanho - self.larg - 0) + self.mini)


    def evento(self, event):
        pos = pygame.mouse.get_pos()
        if event.type == pygame.MOUSEBUTTONDOWN:
               
            if self.x < pos[0] and self.x + self.w > pos[0] and self.y < pos[1] and self.y + self.h > pos[1]:
                self.ativo = True

        if event.type == pygame.MOUSEBUTTONUP:
            self.ativo = False
        if self.ativo:
            if self.horizontal and pos[0] - self.larg/2 >= self.x and pos[0] + self.larg/2 <= self.x + self.tamanho:
                self.x_i = pos[0] - (self.larg/2)
                self.valor = abs(((self.x_i-self.x) - 0) * (self.maxi - self.mini) / (self.tamanho - self.larg - 0) + self.mini)
                        
            elif not self.horizontal and pos[1] - self.larg/2 >= self.y and pos[1] + self.larg/2 <= self.y + self.tamanho:
                self.y_i = pos[1] - (self.larg/2)
                self.valor = abs(((self.y_i-self.y) - 0) * (self.maxi - self.mini) / (self.tamanho - self.larg - 0) + self.mini)
        self.valor = int(self.valor)
        #return self.valor

    def draw(self, fundo):
        #Barra
        pygame.draw.rect(fundo, cinza, [self.x - 3, self.y - 3, self.w + 6, self.h + 6])
        pygame.draw.rect(fundo, self.cor, [self.x, self.y, self.w, self.h])
        if self.horizontal:
            pygame.draw.rect(fundo, preto, [self.x_i, self.y_i, -1*(self.x_i - self.x - self.tamanho), self.larg])
        if not self.horizontal:
            pygame.draw.rect(fundo, preto, [self.x_i, self.y_i, self.larg, -1*(self.y_i - self.y - self.tamanho)])
        pygame.draw.rect(fundo, branco, [self.x_i, self.y_i, self.larg, self.larg])
          
        #Nome
        fundo.blit(FONT.render(f'{self.nome}:{abs(self.valor)}', True, self.cor), (self.x, self.y - 20))
          
        #Intencidade
        #fundo.blit(FONT.render(str(self.valor), True, cinza), (self.x_i, self.y_i))
        
    def getVal(self):
        return self.valor

class Projetar():
    def __init__(self, pontos, arestas, dist, cor, pos):
        self.pontos = pontos       
        self.arestas = arestas
        self.dist = dist
        self.cor = cor
        self.pos = pos
        self.angX = 0
        self.angY = 0
        self.angZ = 0
        
    def setPontos(self, novosPontos, novasArestas):
        self.pontos = novosPontos
        self.arestas = novasArestas

    def Draw(self, tela):
        rodarX = [[1,             0,              0],
                  [0, math.cos(self.angX), -math.sin(self.angX)],
                  [0, math.sin(self.angX),  math.cos(self.angX)]]
            
        rodarY = [[math.cos(self.angY), 0, -math.sin(self.angY)],
                  [0,             1,              0],
                  [math.sin(self.angY), 0,  math.cos(self.angY)]]
            
        rodarZ = [[math.cos(self.angZ), -math.sin(self.angZ), 0],
                  [math.sin(self.angZ),  math.cos(self.angZ), 0],
                  [0,                0,                       1]]
            

        pnts = []
        for pnt in self.pontos:
            ponto = pnt
            ponto = MultMat(rodarX, ponto)
            ponto = MultMat(rodarY, ponto)
            ponto = MultMat(rodarZ, ponto)
            
            z = 1/(ponto[2][0] - self.dist)

            ort = [[z, 0, 0],
                   [0, z, 0]]

            ponto = MultMat(ort, ponto)

            ponto = MultFloat(350, ponto)
            ponto = SomaMat(self.pos, ponto)
            
            ponto = MatPraVec(ponto)
            
            #pygame.draw.circle(tela, verde, (int(ponto[0]),int(ponto[1])), 6)

            pnts.append(ponto)

        i = 0
        for lin in self.arestas:
            pygame.draw.line(tela, self.cor, pnts[lin[0]], pnts[lin[1]], 2)
            i+=1
            
    def Rotacionar(self, angX, angY, angZ):
        self.angX = angX
        self.angY = angY
        self.angZ = angZ


pontosPlano = []

quadro = pygame.Rect(40, 40, largura-40-140, altura-80)
rotacionar = Botao(vermelho_escuro, vermelho, cinza, quadro.x+quadro.width+20, quadro.y, 100, 60, "Rotacionar", "Rotacionar")
numeroFaces = BarraInf("Numero rot", quadro.x+quadro.width+20, quadro.y+100, 100, 20, 1, 24, True, 20, vermelho)

def desenhar_plano():

    sair=False
    while not sair:
        tela.fill(azul_escuro)
        for event in pygame.event.get():
            pos = pygame.mouse.get_pos()
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if pygame.Rect.collidepoint(quadro, pos):
                if pygame.mouse.get_pressed()[0]:
                    novoPnt = True
                    for ponto in pontosPlano:
                        pnt = (int(ponto[0]*quadro.height+40), int(ponto[1]*quadro.height+quadro.height/2+40))
                        if dist(pnt, pos)<=6:
                            novoPonto = ponto
                            novoPnt = False

                    if novoPnt:
                        novoPonto = ((pygame.mouse.get_pos()[0]-40)/quadro.height, (pygame.mouse.get_pos()[1]-40)/quadro.height-0.5) 
                    pontosPlano.append(novoPonto)
                    

                if pygame.mouse.get_pressed()[2] and len(pontosPlano)>0:
                    pontosPlano.pop()
            rotacionar.evento(event)
            numeroFaces.evento(event)

        if rotacionar.getState():
            sair = True
            rotacionar.setState(False)

        rotacionar.draw(tela)
        numeroFaces.draw(tela)
        
        pygame.draw.rect(tela, preto, quadro)
        pygame.draw.rect(tela, cinza, quadro, 1)
        pygame.draw.line(tela, ciano, (quadro.x,quadro.y),(quadro.x, quadro.y+quadro.height))

        instru1 = FONT.render(f'Pontos no quadro serão rotacionados em torno do eixo em ciano', True, azul_escuro2)
        instru3 = FONT.render(f'Os botões direito e esquerdo do mouse criam e apagam pontos no quadro', True, azul_escuro2)
        
        tela.blit(instru1, (quadro.x, 15))
        tela.blit(instru3, (quadro.x, altura-instru3.get_height()-10))
        
        i=0             
        for ponto in pontosPlano:
            pnt = (int(ponto[0]*quadro.height+40), int(ponto[1]*quadro.height+quadro.height/2+40))
            if len(pontosPlano)>i+1:
                pygame.draw.line(tela, verde, pnt, (int(pontosPlano[i+1][0]*quadro.height+40), int(pontosPlano[i+1][1]*quadro.height+quadro.height/2+40)))
                
            elif pygame.Rect.collidepoint(quadro, pygame.mouse.get_pos()):
                pygame.draw.line(tela, verde, pnt, pygame.mouse.get_pos())
                
            pygame.draw.circle(tela, cinza, (int(ponto[0]*(quadro.height)+40), int(ponto[1]*(quadro.height)+quadro.height/2+40)), 7)
            if ponto[0] != 0:
                pygame.draw.circle(tela, verde, (int(ponto[0]*(quadro.height)+40), int(ponto[1]*(quadro.height)+quadro.height/2+40)), 6)
            else:
                pygame.draw.circle(tela, ciano, (int(ponto[0]*(quadro.height)+40), int(ponto[1]*(quadro.height)+quadro.height/2+40)), 6)
                
            i+=1

        pygame.display.update()
    


def observar_solido():
    numeroDeFaces = numeroFaces.getVal()
    pontosSolidos, bordas, faces = gerarSolido(pontosPlano, numeroDeFaces)

    posicao = [[largura/2-80],[altura/2]]

    solidos = []

    Bordas = Projetar(pontosSolidos, bordas, 2, magenta, posicao)
    Faces = Projetar(pontosSolidos, faces, 2, verde, posicao)
    Eixo = Projetar([[[0],[-0.6], [0]],[[0], [0.6], [0]]], [(0,1)], 2, ciano, posicao)

    
    solidos.append(Bordas)
    solidos.append(Faces)
    solidos.append(Eixo)
    angX = pi
    angY = 0
    angZ = 0

    bots = []
    
    botBordas = Botao(magenta_escuro, magenta, cinza, quadro.x+quadro.width+20, quadro.y+100+40, 100, 60, "Borda", "Borda")
    botFaces = Botao(verde_escuro, verde, cinza, quadro.x+quadro.width+20, quadro.y+100+120, 100, 60, "Faces", "Faces")
    botEixo = Botao(ciano_escuro, ciano, cinza, quadro.x+quadro.width+20, quadro.y+100+200, 100, 60, "Eixo", "Eixo")
    botVoltar = Botao(vermelho_escuro, vermelho, cinza, quadro.x+quadro.width+20, quadro.y, 100, 60, "Voltar", "Voltar")

    bots.append(botBordas)
    bots.append(botFaces)
    bots.append(botEixo)
    bots.append(botVoltar)

    vel = 0.01
    
    sair=False
    while not sair:
        tela.fill(azul_escuro)
        for event in pygame.event.get():
  
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
                     
            numeroFaces.evento(event)
            if numeroDeFaces!=numeroFaces.getVal():
                numeroDeFaces = numeroFaces.getVal()
                pontosSolidos, bordas, faces = gerarSolido(pontosPlano, numeroDeFaces)

                Bordas.setPontos(pontosSolidos, bordas)
                Faces.setPontos(pontosSolidos, faces)
            for bot in bots:
                bot.evento(event)
                
        keys = pygame.key.get_pressed()
        if keys[pygame.K_w]:
            angX-=vel
        if keys[pygame.K_a]:
            angY-=vel
        if keys[pygame.K_s]:
            angX+=vel
        if keys[pygame.K_d]:
            angY+=vel
        if keys[pygame.K_RIGHT]:
            angZ+=vel
        if keys[pygame.K_LEFT]:
            angZ-=vel
                
        numeroFaces.draw(tela)

        for bot in bots:
            bot.draw(tela)
        instru2 = FONT.render(f'Gire a figura usando w,a,s,d e as setas', True, azul_escuro2)
        
        tela.blit(instru2, (quadro.x, altura-instru2.get_height()-10))

        for i in range(len(solidos)):
            solidos[i].Rotacionar(angX, angY, angZ)
            if not bots[i].getState():
                solidos[i].Draw(tela)
        if botVoltar.getState():
            sair = True
            botVoltar.setState(False)

        #pygame.time.Clock().tick(24)
        pygame.display.update()
    
        
def Menu():
    exemplos = []
    
    cone = [[[0.0], [-0.32692], [0.0]], [[0.375], [0.32884], [0.0]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[0.32475], [0.32884], [0.18749]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[0.1875], [0.32884], [0.32475]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[2.29621e-17], [0.32884], [0.375]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-0.18749], [0.32884], [0.32475]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-0.32475], [0.32884], [0.18749]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-0.375], [0.32884], [4.59242e-17]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-0.32475], [0.32884], [-0.18749]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-0.1875], [0.32884], [-0.32475]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[-6.88863e-17], [0.32884], [-0.375]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[0.1875], [0.32884], [-0.32475]], [[0.0], [0.32884], [0.0]], [[0.0], [-0.32692], [0.0]], [[0.32475], [0.32884], [-0.1875]], [[0.0], [0.32884], [0.0]]]
    cone_borda = [(0, 3), (1, 4), (2, 5), (3, 6), (4, 7), (5, 8), (6, 9), (7, 10), (8, 11), (9, 12), (10, 13), (11, 14), (12, 15), (13, 16), (14, 17), (15, 18), (16, 19), (17, 20), (18, 21), (19, 22), (20, 23), (21, 24), (22, 25), (23, 26), (24, 27), (25, 28), (26, 29), (27, 30), (28, 31), (29, 32), (30, 33), (31, 34), (32, 35), (33, 0), (34, 1), (35, 2)]
    cone_face = [(0, 1), (1, 2), (3, 4), (4, 5), (6, 7), (7, 8), (9, 10), (10, 11), (12, 13), (13, 14), (15, 16), (16, 17), (18, 19), (19, 20), (21, 22), (22, 23), (24, 25), (25, 26), (27, 28), (28, 29), (30, 31), (31, 32), (33, 34), (34, 35)]
    exemplos.append((cone, cone_borda, cone_face))

    cilindro = [[[0.0], [-0.40961], [0.0]], [[0.57692], [-0.40961], [0.0]], [[0.57692], [0.36538], [0.0]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[0.49963], [-0.40961], [0.28846]], [[0.49963], [0.36538], [0.28846]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[0.28846], [-0.40961], [0.49963]], [[0.28846], [0.36538], [0.49963]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[3.53263e-17], [-0.40961], [0.57692]], [[3.53263e-17], [0.36538], [0.57692]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-0.28846], [-0.40961], [0.49963]], [[-0.28846], [0.36538], [0.49963]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-0.49963], [-0.40961], [0.28846]], [[-0.49963], [0.36538], [0.28846]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-0.57692], [-0.40961], [7.06526e-17]], [[-0.57692], [0.36538], [7.06526e-17]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-0.49963], [-0.40961], [-0.28846]], [[-0.49963], [0.36538], [-0.28846]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-0.28846], [-0.40961], [-0.49963]], [[-0.28846], [0.36538], [-0.49963]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[-1.05979e-16], [-0.40961], [-0.57692]], [[-1.05979e-16], [0.36538], [-0.57692]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[0.28846], [-0.40961], [-0.49963]], [[0.28846], [0.36538], [-0.49963]], [[0.0], [0.36538], [0.0]], [[0.0], [-0.40961], [0.0]], [[0.49963], [-0.40961], [-0.28846]], [[0.49963], [0.36538], [-0.28846]], [[0.0], [0.36538], [0.0]]]
    cilindro_borda = [(0, 4), (1, 5), (2, 6), (3, 7), (4, 8), (5, 9), (6, 10), (7, 11), (8, 12), (9, 13), (10, 14), (11, 15), (12, 16), (13, 17), (14, 18), (15, 19), (16, 20), (17, 21), (18, 22), (19, 23), (20, 24), (21, 25), (22, 26), (23, 27), (24, 28), (25, 29), (26, 30), (27, 31), (28, 32), (29, 33), (30, 34), (31, 35), (32, 36), (33, 37), (34, 38), (35, 39), (36, 40), (37, 41), (38, 42), (39, 43), (40, 44), (41, 45), (42, 46), (43, 47), (44, 0), (45, 1), (46, 2), (47, 3)]
    cilindro_face = [(0, 1), (1, 2), (2, 3), (4, 5), (5, 6), (6, 7), (8, 9), (9, 10), (10, 11), (12, 13), (13, 14), (14, 15), (16, 17), (17, 18), (18, 19), (20, 21), (21, 22), (22, 23), (24, 25), (25, 26), (26, 27), (28, 29), (29, 30), (30, 31), (32, 33), (33, 34), (34, 35), (36, 37), (37, 38), (38, 39), (40, 41), (41, 42), (42, 43), (44, 45), (45, 46), (46, 47)]
    exemplos.append((cilindro, cilindro_borda, cilindro_face))
    
    circunferência = [[[0.0], [-0.38461], [0.0]], [[0.14615], [-0.35384], [0.0]], [[0.26153], [-0.28269], [0.0]], [[0.34615], [-0.16153], [0.0]], [[0.38269], [-0.00384], [0.0]], [[0.35192], [0.14615], [0.0]], [[0.2923], [0.25], [0.0]], [[0.175], [0.34038], [0.0]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[0.12657], [-0.35384], [0.07307]], [[0.22649], [-0.28269], [0.13076]], [[0.29977], [-0.16153], [0.17307]], [[0.33142], [-0.00384], [0.19134]], [[0.30477], [0.14615], [0.17596]], [[0.25314], [0.25], [0.14615]], [[0.15155], [0.34038], [0.08749]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[0.07307], [-0.35384], [0.12657]], [[0.13076], [-0.28269], [0.22649]], [[0.17307], [-0.16153], [0.29977]], [[0.19134], [-0.00384], [0.33142]], [[0.17596], [0.14615], [0.30477]], [[0.14615], [0.25], [0.25314]], [[0.0875], [0.34038], [0.15155]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[8.94934e-18], [-0.35384], [0.14615]], [[1.60146e-17], [-0.28269], [0.26153]], [[2.11958e-17], [-0.16153], [0.34615]], [[2.34331e-17], [-0.00384], [0.38269]], [[2.1549e-17], [0.14615], [0.35192]], [[1.78986e-17], [0.25], [0.2923]], [[1.07156e-17], [0.34038], [0.175]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-0.07307], [-0.35384], [0.12657]], [[-0.13076], [-0.28269], [0.22649]], [[-0.17307], [-0.16153], [0.29977]], [[-0.19134], [-0.00384], [0.33142]], [[-0.17596], [0.14615], [0.30477]], [[-0.14615], [0.25], [0.25314]], [[-0.08749], [0.34038], [0.15155]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-0.12657], [-0.35384], [0.07307]], [[-0.22649], [-0.28269], [0.13076]], [[-0.29977], [-0.16153], [0.17307]], [[-0.33142], [-0.00384], [0.19134]], [[-0.30477], [0.14615], [0.17596]], [[-0.25314], [0.25], [0.14615]], [[-0.15155], [0.34038], [0.08749]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-0.14615], [-0.35384], [1.78986e-17]], [[-0.26153], [-0.28269], [3.20292e-17]], [[-0.34615], [-0.16153], [4.23916e-17]], [[-0.38269], [-0.00384], [4.68662e-17]], [[-0.35192], [0.14615], [4.30981e-17]], [[-0.2923], [0.25], [3.57973e-17]], [[-0.175], [0.34038], [2.14313e-17]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-0.12657], [-0.35384], [-0.07307]], [[-0.22649], [-0.28269], [-0.13076]], [[-0.29977], [-0.16153], [-0.17307]], [[-0.33142], [-0.00384], [-0.19134]], [[-0.30477], [0.14615], [-0.17596]], [[-0.25314], [0.25], [-0.14615]], [[-0.15155], [0.34038], [-0.08749]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-0.07307], [-0.35384], [-0.12657]], [[-0.13076], [-0.28269], [-0.22649]], [[-0.17307], [-0.16153], [-0.29977]], [[-0.19134], [-0.00384], [-0.33142]], [[-0.17596], [0.14615], [-0.30477]], [[-0.14615], [0.25], [-0.25314]], [[-0.0875], [0.34038], [-0.15155]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[-2.6848e-17], [-0.35384], [-0.14615]], [[-4.80438e-17], [-0.28269], [-0.26153]], [[-6.35874e-17], [-0.16153], [-0.34615]], [[-7.02994e-17], [-0.00384], [-0.38269]], [[-6.46472e-17], [0.14615], [-0.35192]], [[-5.3696e-17], [0.25], [-0.2923]], [[-3.21469e-17], [0.34038], [-0.175]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[0.07307], [-0.35384], [-0.12657]], [[0.13076], [-0.28269], [-0.22649]], [[0.17307], [-0.16153], [-0.29977]], [[0.19134], [-0.00384], [-0.33142]], [[0.17596], [0.14615], [-0.30477]], [[0.14615], [0.25], [-0.25314]], [[0.0875], [0.34038], [-0.15155]], [[0.0], [0.38269], [0.0]], [[0.0], [-0.38461], [0.0]], [[0.12657], [-0.35384], [-0.07307]], [[0.22649], [-0.28269], [-0.13076]], [[0.29977], [-0.16153], [-0.17307]], [[0.33142], [-0.00384], [-0.19134]], [[0.30477], [0.14615], [-0.17596]], [[0.25314], [0.25], [-0.14615]], [[0.15155], [0.34038], [-0.0875]], [[0.0], [0.38269], [0.0]]]
    circunferência_borda = [(0, 9), (1, 10), (2, 11), (3, 12), (4, 13), (5, 14), (6, 15), (7, 16), (8, 17), (9, 18), (10, 19), (11, 20), (12, 21), (13, 22), (14, 23), (15, 24), (16, 25), (17, 26), (18, 27), (19, 28), (20, 29), (21, 30), (22, 31), (23, 32), (24, 33), (25, 34), (26, 35), (27, 36), (28, 37), (29, 38), (30, 39), (31, 40), (32, 41), (33, 42), (34, 43), (35, 44), (36, 45), (37, 46), (38, 47), (39, 48), (40, 49), (41, 50), (42, 51), (43, 52), (44, 53), (45, 54), (46, 55), (47, 56), (48, 57), (49, 58), (50, 59), (51, 60), (52, 61), (53, 62), (54, 63), (55, 64), (56, 65), (57, 66), (58, 67), (59, 68), (60, 69), (61, 70), (62, 71), (63, 72), (64, 73), (65, 74), (66, 75), (67, 76), (68, 77), (69, 78), (70, 79), (71, 80), (72, 81), (73, 82), (74, 83), (75, 84), (76, 85), (77, 86), (78, 87), (79, 88), (80, 89), (81, 90), (82, 91), (83, 92), (84, 93), (85, 94), (86, 95), (87, 96), (88, 97), (89, 98), (90, 99), (91, 100), (92, 101), (93, 102), (94, 103), (95, 104), (96, 105), (97, 106), (98, 107), (99, 0), (100, 1), (101, 2), (102, 3), (103, 4), (104, 5), (105, 6), (106, 7), (107, 8)]
    circunferência_face = [(0, 1), (1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8), (9, 10), (10, 11), (11, 12), (12, 13), (13, 14), (14, 15), (15, 16), (16, 17), (18, 19), (19, 20), (20, 21), (21, 22), (22, 23), (23, 24), (24, 25), (25, 26), (27, 28), (28, 29), (29, 30), (30, 31), (31, 32), (32, 33), (33, 34), (34, 35), (36, 37), (37, 38), (38, 39), (39, 40), (40, 41), (41, 42), (42, 43), (43, 44), (45, 46), (46, 47), (47, 48), (48, 49), (49, 50), (50, 51), (51, 52), (52, 53), (54, 55), (55, 56), (56, 57), (57, 58), (58, 59), (59, 60), (60, 61), (61, 62), (63, 64), (64, 65), (65, 66), (66, 67), (67, 68), (68, 69), (69, 70), (70, 71), (72, 73), (73, 74), (74, 75), (75, 76), (76, 77), (77, 78), (78, 79), (79, 80), (81, 82), (82, 83), (83, 84), (84, 85), (85, 86), (86, 87), (87, 88), (88, 89), (90, 91), (91, 92), (92, 93), (93, 94), (94, 95), (95, 96), (96, 97), (97, 98), (99, 100), (100, 101), (101, 102), (102, 103), (103, 104), (104, 105), (105, 106), (106, 107)]
    exemplos.append((circunferência, circunferência_borda, circunferência_face))
    
    toro = [[[0.63653], [-0.26923], [0.0]], [[0.77115], [-0.23076], [0.0]], [[0.86923], [-0.13076], [0.0]], [[0.90192], [0.00769], [0.0]], [[0.86923], [0.12884], [0.0]], [[0.78653], [0.21923], [0.0]], [[0.63461], [0.26923], [0.0]], [[0.49807], [0.23076], [0.0]], [[0.40576], [0.14038], [0.0]], [[0.36538], [0.00576], [0.0]], [[0.4], [-0.13076], [0.0]], [[0.4923], [-0.22884], [0.0]], [[0.63653], [-0.26923], [0.0]], [[0.55125], [-0.26923], [0.31826]], [[0.66783], [-0.23076], [0.38557]], [[0.75277], [-0.13076], [0.43461]], [[0.78108], [0.00769], [0.45096]], [[0.75277], [0.12884], [0.43461]], [[0.68116], [0.21923], [0.39326]], [[0.54959], [0.26923], [0.3173]], [[0.43134], [0.23076], [0.24903]], [[0.3514], [0.14038], [0.20288]], [[0.31643], [0.00576], [0.18269]], [[0.34641], [-0.13076], [0.19999]], [[0.42635], [-0.22884], [0.24615]], [[0.55125], [-0.26923], [0.31826]], [[0.31826], [-0.26923], [0.55125]], [[0.38557], [-0.23076], [0.66783]], [[0.43461], [-0.13076], [0.75277]], [[0.45096], [0.00769], [0.78108]], [[0.43461], [0.12884], [0.75277]], [[0.39326], [0.21923], [0.68116]], [[0.3173], [0.26923], [0.54959]], [[0.24903], [0.23076], [0.43134]], [[0.20288], [0.14038], [0.3514]], [[0.18269], [0.00576], [0.31643]], [[0.2], [-0.13076], [0.34641]], [[0.24615], [-0.22884], [0.42635]], [[0.31826], [-0.26923], [0.55125]], [[3.89767e-17], [-0.26923], [0.63653]], [[4.72195e-17], [-0.23076], [0.77115]], [[5.3225e-17], [-0.13076], [0.86923]], [[5.52268e-17], [0.00769], [0.90192]], [[5.3225e-17], [0.12884], [0.86923]], [[4.81615e-17], [0.21923], [0.78653]], [[3.88589e-17], [0.26923], [0.63461]], [[3.04984e-17], [0.23076], [0.49807]], [[2.48461e-17], [0.14038], [0.40576]], [[2.23733e-17], [0.00576], [0.36538]], [[2.44929e-17], [-0.13076], [0.4]], [[3.01451e-17], [-0.22884], [0.4923]], [[3.89767e-17], [-0.26923], [0.63653]], [[-0.31826], [-0.26923], [0.55125]], [[-0.38557], [-0.23076], [0.66783]], [[-0.43461], [-0.13076], [0.75277]], [[-0.45096], [0.00769], [0.78108]], [[-0.43461], [0.12884], [0.75277]], [[-0.39326], [0.21923], [0.68116]], [[-0.3173], [0.26923], [0.54959]], [[-0.24903], [0.23076], [0.43134]], [[-0.20288], [0.14038], [0.3514]], [[-0.18269], [0.00576], [0.31643]], [[-0.19999], [-0.13076], [0.34641]], [[-0.24615], [-0.22884], [0.42635]], [[-0.31826], [-0.26923], [0.55125]], [[-0.55125], [-0.26923], [0.31826]], [[-0.66783], [-0.23076], [0.38557]], [[-0.75277], [-0.13076], [0.43461]], [[-0.78108], [0.00769], [0.45096]], [[-0.75277], [0.12884], [0.43461]], [[-0.68116], [0.21923], [0.39326]], [[-0.54959], [0.26923], [0.3173]], [[-0.43134], [0.23076], [0.24903]], [[-0.3514], [0.14038], [0.20288]], [[-0.31643], [0.00576], [0.18269]], [[-0.34641], [-0.13076], [0.19999]], [[-0.42635], [-0.22884], [0.24615]], [[-0.55125], [-0.26923], [0.31826]], [[-0.63653], [-0.26923], [7.79534e-17]], [[-0.77115], [-0.23076], [9.44391e-17]], [[-0.86923], [-0.13076], [1.0645e-16]], [[-0.90192], [0.00769], [1.10453e-16]], [[-0.86923], [0.12884], [1.0645e-16]], [[-0.78653], [0.21923], [9.63231e-17]], [[-0.63461], [0.26923], [7.77179e-17]], [[-0.49807], [0.23076], [6.09968e-17]], [[-0.40576], [0.14038], [4.96923e-17]], [[-0.36538], [0.00576], [4.47467e-17]], [[-0.4], [-0.13076], [4.89858e-17]], [[-0.4923], [-0.22884], [6.02903e-17]], [[-0.63653], [-0.26923], [7.79534e-17]], [[-0.55125], [-0.26923], [-0.31826]], [[-0.66783], [-0.23076], [-0.38557]], [[-0.75277], [-0.13076], [-0.43461]], [[-0.78108], [0.00769], [-0.45096]], [[-0.75277], [0.12884], [-0.43461]], [[-0.68116], [0.21923], [-0.39326]], [[-0.54959], [0.26923], [-0.3173]], [[-0.43134], [0.23076], [-0.24903]], [[-0.3514], [0.14038], [-0.20288]], [[-0.31643], [0.00576], [-0.18269]], [[-0.34641], [-0.13076], [-0.19999]], [[-0.42635], [-0.22884], [-0.24615]], [[-0.55125], [-0.26923], [-0.31826]], [[-0.31826], [-0.26923], [-0.55125]], [[-0.38557], [-0.23076], [-0.66783]], [[-0.43461], [-0.13076], [-0.75277]], [[-0.45096], [0.00769], [-0.78108]], [[-0.43461], [0.12884], [-0.75277]], [[-0.39326], [0.21923], [-0.68116]], [[-0.3173], [0.26923], [-0.54959]], [[-0.24903], [0.23076], [-0.43134]], [[-0.20288], [0.14038], [-0.3514]], [[-0.18269], [0.00576], [-0.31643]], [[-0.2], [-0.13076], [-0.34641]], [[-0.24615], [-0.22884], [-0.42635]], [[-0.31826], [-0.26923], [-0.55125]], [[-1.1693e-16], [-0.26923], [-0.63653]], [[-1.41658e-16], [-0.23076], [-0.77115]], [[-1.59675e-16], [-0.13076], [-0.86923]], [[-1.6568e-16], [0.00769], [-0.90192]], [[-1.59675e-16], [0.12884], [-0.86923]], [[-1.44484e-16], [0.21923], [-0.78653]], [[-1.16576e-16], [0.26923], [-0.63461]], [[-9.14952e-17], [0.23076], [-0.49807]], [[-7.45385e-17], [0.14038], [-0.40576]], [[-6.712e-17], [0.00576], [-0.36538]], [[-7.34788e-17], [-0.13076], [-0.4]], [[-9.04354e-17], [-0.22884], [-0.4923]], [[-1.1693e-16], [-0.26923], [-0.63653]], [[0.31826], [-0.26923], [-0.55125]], [[0.38557], [-0.23076], [-0.66783]], [[0.43461], [-0.13076], [-0.75277]], [[0.45096], [0.00769], [-0.78108]], [[0.43461], [0.12884], [-0.75277]], [[0.39326], [0.21923], [-0.68116]], [[0.3173], [0.26923], [-0.54959]], [[0.24903], [0.23076], [-0.43134]], [[0.20288], [0.14038], [-0.3514]], [[0.18269], [0.00576], [-0.31643]], [[0.2], [-0.13076], [-0.34641]], [[0.24615], [-0.22884], [-0.42635]], [[0.31826], [-0.26923], [-0.55125]], [[0.55125], [-0.26923], [-0.31826]], [[0.66783], [-0.23076], [-0.38557]], [[0.75277], [-0.13076], [-0.43461]], [[0.78108], [0.00769], [-0.45096]], [[0.75277], [0.12884], [-0.43461]], [[0.68116], [0.21923], [-0.39326]], [[0.54959], [0.26923], [-0.3173]], [[0.43134], [0.23076], [-0.24903]], [[0.3514], [0.14038], [-0.20288]], [[0.31643], [0.00576], [-0.18269]], [[0.34641], [-0.13076], [-0.2]], [[0.42635], [-0.22884], [-0.24615]], [[0.55125], [-0.26923], [-0.31826]]]
    toro_borda = [(0, 13), (1, 14), (2, 15), (3, 16), (4, 17), (5, 18), (6, 19), (7, 20), (8, 21), (9, 22), (10, 23), (11, 24), (12, 25), (13, 26), (14, 27), (15, 28), (16, 29), (17, 30), (18, 31), (19, 32), (20, 33), (21, 34), (22, 35), (23, 36), (24, 37), (25, 38), (26, 39), (27, 40), (28, 41), (29, 42), (30, 43), (31, 44), (32, 45), (33, 46), (34, 47), (35, 48), (36, 49), (37, 50), (38, 51), (39, 52), (40, 53), (41, 54), (42, 55), (43, 56), (44, 57), (45, 58), (46, 59), (47, 60), (48, 61), (49, 62), (50, 63), (51, 64), (52, 65), (53, 66), (54, 67), (55, 68), (56, 69), (57, 70), (58, 71), (59, 72), (60, 73), (61, 74), (62, 75), (63, 76), (64, 77), (65, 78), (66, 79), (67, 80), (68, 81), (69, 82), (70, 83), (71, 84), (72, 85), (73, 86), (74, 87), (75, 88), (76, 89), (77, 90), (78, 91), (79, 92), (80, 93), (81, 94), (82, 95), (83, 96), (84, 97), (85, 98), (86, 99), (87, 100), (88, 101), (89, 102), (90, 103), (91, 104), (92, 105), (93, 106), (94, 107), (95, 108), (96, 109), (97, 110), (98, 111), (99, 112), (100, 113), (101, 114), (102, 115), (103, 116), (104, 117), (105, 118), (106, 119), (107, 120), (108, 121), (109, 122), (110, 123), (111, 124), (112, 125), (113, 126), (114, 127), (115, 128), (116, 129), (117, 130), (118, 131), (119, 132), (120, 133), (121, 134), (122, 135), (123, 136), (124, 137), (125, 138), (126, 139), (127, 140), (128, 141), (129, 142), (130, 143), (131, 144), (132, 145), (133, 146), (134, 147), (135, 148), (136, 149), (137, 150), (138, 151), (139, 152), (140, 153), (141, 154), (142, 155), (143, 0), (144, 1), (145, 2), (146, 3), (147, 4), (148, 5), (149, 6), (150, 7), (151, 8), (152, 9), (153, 10), (154, 11), (155, 12)]
    toro_face = [(0, 1), (1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7), (7, 8), (8, 9), (9, 10), (10, 11), (11, 12), (13, 14), (14, 15), (15, 16), (16, 17), (17, 18), (18, 19), (19, 20), (20, 21), (21, 22), (22, 23), (23, 24), (24, 25), (26, 27), (27, 28), (28, 29), (29, 30), (30, 31), (31, 32), (32, 33), (33, 34), (34, 35), (35, 36), (36, 37), (37, 38), (39, 40), (40, 41), (41, 42), (42, 43), (43, 44), (44, 45), (45, 46), (46, 47), (47, 48), (48, 49), (49, 50), (50, 51), (52, 53), (53, 54), (54, 55), (55, 56), (56, 57), (57, 58), (58, 59), (59, 60), (60, 61), (61, 62), (62, 63), (63, 64), (65, 66), (66, 67), (67, 68), (68, 69), (69, 70), (70, 71), (71, 72), (72, 73), (73, 74), (74, 75), (75, 76), (76, 77), (78, 79), (79, 80), (80, 81), (81, 82), (82, 83), (83, 84), (84, 85), (85, 86), (86, 87), (87, 88), (88, 89), (89, 90), (91, 92), (92, 93), (93, 94), (94, 95), (95, 96), (96, 97), (97, 98), (98, 99), (99, 100), (100, 101), (101, 102), (102, 103), (104, 105), (105, 106), (106, 107), (107, 108), (108, 109), (109, 110), (110, 111), (111, 112), (112, 113), (113, 114), (114, 115), (115, 116), (117, 118), (118, 119), (119, 120), (120, 121), (121, 122), (122, 123), (123, 124), (124, 125), (125, 126), (126, 127), (127, 128), (128, 129), (130, 131), (131, 132), (132, 133), (133, 134), (134, 135), (135, 136), (136, 137), (137, 138), (138, 139), (139, 140), (140, 141), (141, 142), (143, 144), (144, 145), (145, 146), (146, 147), (147, 148), (148, 149), (149, 150), (150, 151), (151, 152), (152, 153), (153, 154), (154, 155)]
    exemplos.append((toro, toro_borda, toro_face))


    exemplo = exemplos[random.randint(1, len(exemplos))-1]
    posicao = [[largura*2/3],[altura/2]]

    Solido_borda = Projetar(exemplo[0], exemplo[1], 2, magenta, posicao)
    Solido_faces = Projetar(exemplo[0], exemplo[2], 2, verde, posicao)

    angX = 0
    angY = 0
    angZ = 0
    
    velX = random.random()*0.01
    velY = random.random()*0.01
    velZ = random.random()*0.01
    
    Iniciar = Botao(verde_escuro, verde, cinza, int((largura-100)/3), int((altura-60)/2), 100, 60, "Iniciar", "Iniciar")
    sair=False
    while not sair:
        tela.fill(azul_escuro)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            Iniciar.evento(event)
        Iniciar.draw(tela)
        Solido_borda.Draw(tela)
        Solido_faces.Draw(tela)
        Solido_borda.Rotacionar(angX, angY, angZ)
        Solido_faces.Rotacionar(angX, angY, angZ)
        angX += velX
        angY += velY
        angZ += velZ
        
        título=FONT1.render(f'Sólidos de Revolução', True, magenta)
        nome = FONT.render(f'Feito por: Tomás Ribeiro', True, azul_escuro2)
        data = FONT.render(f'2020', True, azul_escuro2)
        
        tela.blit(título, (largura/2-título.get_width()/2, altura/5))
        tela.blit(nome, (largura/3-nome.get_width()/2, altura*4/5))
        tela.blit(data, (largura*2/3-data.get_width()/2, altura*4/5))

        if Iniciar.getState():
            sair=True
            Iniciar.setState(False)
        pygame.display.update()

def main():
    Menu()
    sair=False
    while not sair:
        tela.fill(azul_escuro)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sair = True

        desenhar_plano()
        observar_solido()


        pygame.display.update()
    pygame.quit()

if __name__ == "__main__":
    main()

