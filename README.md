# Instagram
!/usr/bin/env python3

import requests
import random
import string
import socket
import os

def limpar():
    os.system('clear' if os.name == 'posix' else 'cls')

def banner():
    print("""
=========================================
        ETHICAL - CLI PAINEL
        By Cleverson Gomes
=========================================
""")

def menu():
    print("""
[1] Verificar status HTTP de um site
[2] Gerar senha segura
[3] Verificar portas abertas em host
[4] Ver meu IP público
[5] Extrair strings de arquivo binário
[6] Desmontar script Python
[0] Sair
""")

def status_site():
    url = input("Digite a URL (ex.: https://www.google.com): ")
    try:
        r = requests.get(url, timeout=5)
        print(f"Status Code: {r.status_code}")
    except Exception as e:
        print(f"Erro ao acessar: {e}")

def gerar_senha():
    tamanho = int(input("Tamanho da senha: "))
    caracteres = string.ascii_letters + string.digits + string.punctuation
    senha = ''.join(random.choice(caracteres) for _ in range(tamanho))
    print(f"Senha gerada: {senha}")

def scan_portas():
    host = input("Digite o host (ex.: scanme.nmap.org): ")
    portas = [21, 22, 23, 25, 53, 80, 443, 3306, 8080]
    print(f"Escaneando portas em {host}...")
    for porta in portas:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((host, porta))
        if result == 0:
            print(f"[OPEN] Porta {porta}")
        else:
            print(f"[CLOSED] Porta {porta}")
        sock.close()

def meu_ip():
    try:
        ip = requests.get("https://api.ipify.org").text
        print(f"Seu IP público é: {ip}")
    except:
        print("Erro ao buscar IP.")

def extrair_strings():
    caminho = input("Caminho do arquivo: ")
    try:
        with open(caminho, "rb") as arq:
            dados = arq.read()
        atual = ""
        for byte in dados:
            char = chr(byte)
            if char in string.printable and char not in '\n\r\t\x0b\x0c':
                atual += char
            else:
                if len(atual) >= 4:
                    print(atual)
                atual = ""
        if len(atual) >= 4:
            print(atual)
    except FileNotFoundError:
        print("Arquivo não encontrado.")
    except Exception as e:
        print(f"Erro: {e}")

def desmontar_python():
    caminho = input("Caminho do script Python: ")
    try:
        with open(caminho, "r") as arq:
            source = arq.read()
        import dis
        codigo = compile(source, caminho, 'exec')
        dis.dis(codigo)
    except FileNotFoundError:
        print("Arquivo não encontrado.")
    except Exception as e:
        print(f"Erro: {e}")

def main():
    while True:
        limpar()
        banner()
        menu()
        op = input("Escolha: ")
        if op == '1':
            status_site()
        elif op == '2':
            gerar_senha()
        elif op == '3':
            scan_portas()
        elif op == '4':
            meu_ip()
        elif op == '5':
            extrair_strings()
        elif op == '6':
            desmontar_python()
        elif op == '0':
            print("Saindo...")
            break
        else:
            print("Opção inválida.")
        input("\nPressione Enter para continuar...")

if __name__ == "__main__":
    main()
#
