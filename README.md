import random

# Classe para representar uma carta
class Carta:
    def __init__(self, nome, ataque, vida):
        self.nome = nome
        self.ataque = ataque
        self.vida = vida

    def __str__(self):
        return f"{self.nome} (ATK: {self.ataque}, HP: {self.vida})"

# Classe para representar o jogador
class Jogador:
    def __init__(self, nome):
        self.nome = nome
        self.deck = []
        self.mao = []
        self.campo = None  # Carta ativa

    def comprar_carta(self):
        if self.deck:
            carta = self.deck.pop()
            self.mao.append(carta)
            print(f"{self.nome} comprou {carta}")

    def jogar_carta(self, indice):
        if 0 <= indice < len(self.mao):
            self.campo = self.mao.pop(indice)
            print(f"{self.nome} jogou {self.campo} no campo!")

# Função para criar um deck básico
def criar_deck():
    cartas = [
        Carta("Charmander", 70, 50),
        Carta("Zoroa", 50, 60),
        Carta("Onix", 30, 80),
        Carta("Pikachu", 60, 40),
        Carta("Bulbasaur", 40, 70),  # Nova carta
        Carta("Squirtle", 50, 60)    # Nova carta
    ]
    return cartas

# Função principal do jogo
def jogo():
    print("Bem-vindo ao Pokemon Card Game!")
    
    # Criando os jogadores
    jogador = Jogador("Jogador")
    bot = Jogador("Bot")
    
    # Definindo os decks
    jogador.deck = criar_deck()
    bot.deck = criar_deck().copy()
    random.shuffle(jogador.deck)
    random.shuffle(bot.deck)
    
    # Comprando cartas iniciais
    for _ in range(3):  # Cada um começa com 3 cartas na mão
        jogador.comprar_carta()
        bot.comprar_carta()
    
    # Loop básico de turnos
    turno = 1
    while True:
        print(f"\nTurno {turno} - {jogador.nome}")
        print(f"Sua mão: {[str(carta) for carta in jogador.mao]}")
        print(f"Campo do Bot: {bot.campo if bot.campo else 'Vazio'}")
        
        # Escolha do jogador
        acao = input("Escolha: [1] Jogar carta, [2] Passar: ")
        if acao == "1":
            indice = int(input("Qual carta (0, 1, 2...)? "))
            if 0 <= indice < len(jogador.mao):
                jogador.jogar_carta(indice)
                if bot.campo:
                    bot.campo.vida -= jogador.campo.ataque
                    print(f"{jogador.campo.nome} ataca! {bot.campo.nome} agora tem {bot.campo.vida} HP")
                    if bot.campo.vida <= 0:
                        print(f"{bot.campo.nome} foi derrotado!")
                        bot.campo = None
        
        # Turno do bot 
        if not bot.campo and bot.mao:
            bot.jogar_carta(0)
        elif jogador.campo and bot.campo:
            jogador.campo.vida -= bot.campo.ataque
            print(f"{bot.campo.nome} ataca! {jogador.campo.nome} agora tem {jogador.campo.vida} HP")
            if jogador.campo.vida <= 0:
                print(f"{jogador.campo.nome} foi derrotado!")
                jogador.campo = None
        
        # Condição de vitória 
        if not bot.mao and not bot.campo:
            print("Você venceu!")
            break
        if not jogador.mao and not jogador.campo:
            print("Bot venceu!")
            break
        
        turno += 1

# Rodando o jogo
if __name__ == "__main__":
    jogo()
