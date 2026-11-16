# faculdade-
primeiros códigos

import random
import time

random.seed(2026)


def gerar_token(digitos=6):
    return "".join(str(random.randint(0, 9)) for _ in range(digitos))


def autenticar():
    token = gerar_token()
    tentativas = 3
    inicio = time.time()

    print("\nAUTENTICACAO MANUAL ")
    print("O token tem 6 digitos.")
    print("Voce tem 3 tentativas e 30 segundos.")

    while tentativas > 0:
        if time.time() - inicio >= 30:
            print("\nTempo esgotado! Acesso bloqueado.")
            return

        chute = input("\nDigite o token de 6 digitos: ")

        if time.time() - inicio >= 30:
            print("Tempo esgotado! Acesso bloqueado.")
            return

        if len(chute) != 6 or not chute.isdigit():
            print("Digite exatamente 6 numeros.")
            continue

        tentativas -= 1

        if chute == token:
            print("Token correto! Acesso liberado.")
            return

        print("Token incorreto.")
        print("Tentativas restantes:", tentativas)

    print("\nAcesso bloqueado.")
    print("Token correto para conferência:", token)



def simular_sessao(digitos, tentativas):
    total = 10 ** digitos
    token = random.randrange(total)
    limite = min(tentativas, total)
    chutes = random.sample(range(total), limite)

    inicio = time.time()

    for chute in chutes:
        if time.time() - inicio >= 30:
            return False

        if chute == token:
            return True

    return False



def simular(digitos, tentativas, sessoes):
    acertos = 0

    for _ in range(sessoes):
        if simular_sessao(digitos, tentativas):
            acertos += 1

    prob_empirica = acertos / sessoes
    prob_teorica = min(tentativas, 10 ** digitos) / (10 ** digitos)

    print("\nDigitos do token:", digitos)
    print("Tentativas por sessao:", tentativas)
    print("Sessoes simuladas:", sessoes)
    print("Invasoes bem-sucedidas:", acertos)
    print(f"Probabilidade empirica: {prob_empirica:.8%}")
    print(f"Probabilidade teorica:  {prob_teorica:.8%}")


def menu_simulacao():
    print("\n=== SIMULACAO MATEMATICA ===")
    print("1 - Cenario principal: 6 digitos e 3 tentativas")
    print("2 - Comparar quantidade de digitos")
    print("3 - Comparar quantidade de tentativas")

    opcao = input("Escolha: ")

    if opcao == "1":
        simular(6, 3, 1_000_000)

    elif opcao == "2":
        for digitos in (2, 3, 4, 5, 6):
            simular(digitos, 3, 100_000)

    elif opcao == "3":
        for tentativas in (1, 2, 3):
            simular(6, tentativas, 1_000_000)

    else:
        print("Opcao invalida.")


def main():
    while True:
        print("\n==============================")
        print(" SISTEMA DE TOKENS NUMERICOS")
        print("==============================")
        print("1 - Testar autenticacao manual")
        print("2 - Executar simulacoes matematicas")
        print("3 - Sair")

        opcao = input("Escolha uma opcao: ")

        if opcao == "1":
            autenticar()
        elif opcao == "2":
            menu_simulacao()
        elif opcao == "3":
            print("Programa encerrado.")
            break
        else:
            print("Opcao invalida. Tente novamente.")


if __name__ == "__main__":
    main()
