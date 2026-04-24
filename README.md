# PyGame---Jogo-do-Rato
# Link JuiceMind --- https://play.juicemind.com/sandbox/9d4q7spqhBuxHaMbWwmY
# Código
import os
import pygame
import random

pygame.init()
tela = pygame.display.set_mode((600, 400))
pygame.display.set_caption("Jogo do Rato")

rato = [(100, 50)]
direcao = (10, 0)
sabao = (300, 200)

# carregar imagens
caminho_rato = os.path.join("imagens", "rato.png")
imagem_rato = pygame.image.load(caminho_rato)
imagem_rato = pygame.transform.scale(imagem_rato, (50,50))

caminho_sabao = os.path.join("imagens", "sabao.png")
imagem_sabao = pygame.image.load(caminho_sabao)
imagem_sabao = pygame.transform.scale(imagem_sabao, (50, 50))


def desenhar():
    tela.fill((0, 0, 0))

    for parte in rato:
        tela.blit(imagem_rato, parte)

    tela.blit(imagem_sabao, sabao)

    pygame.display.update()


rodando = True
relogio = pygame.time.Clock()

while rodando:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            rodando = False

        if evento.type == pygame.KEYDOWN:
            if evento.key == pygame.K_UP:
                direcao = (0, -10)
            elif evento.key == pygame.K_DOWN:
                direcao = (0, 10)
            elif evento.key == pygame.K_LEFT:
                direcao = (-10, 0)
            elif evento.key == pygame.K_RIGHT:
                direcao = (10, 0)

    # movimento do rato
    cabeca = (rato[0][0] + direcao[0], rato[0][1] + direcao[1])
    rato.insert(0, cabeca)
    rato.pop()

    # colisão com parede
    if cabeca[0] < 0 or cabeca[0] >= 600 or cabeca[1] < 0 or cabeca[1] >= 400:
        rodando = False

    # colisão com sabão
    if rato[0] == sabao:
        sabao = (random.randrange(0, 600, 10), random.randrange(0, 400, 10))

    desenhar()
    relogio.tick(15)

pygame.quit()
