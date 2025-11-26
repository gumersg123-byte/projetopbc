import json
import os

ARQUIVOS = {
    'projetos': 'data/projetos.json',
    'usuarios': 'data/usuarios.json',
    'tarefas': 'data/tarefas.json'
}

def _verificar_e_criar_pasta(caminho):
    diretorio = os.path.dirname(caminho)
    if diretorio and not os.path.exists(diretorio):
        os.makedirs(diretorio)

def carregar_dados(modulo):
    caminho_arquivo = ARQUIVOS.get(modulo)
    
    if os.path.exists(caminho_arquivo):
        with open(caminho_arquivo, 'r') as f:
            try:
                dados_completos = json.load(f)
                
                dados_dict = dados_completos.get("dados", {})
                proximo_id = dados_completos.get("proximo_id", 1)
                
                return dados_dict, proximo_id
            except json.JSONDecodeError:
                print(f"Erro ao ler o arquivo {caminho_arquivo}. Arquivo vazio ou corrompido.")
                return {}, 1
    
    return {}, 1

def salvar_dados(modulo, dados_dict, proximo_id):
    caminho_arquivo = ARQUIVOS.get(modulo)
    
    _verificar_e_criar_pasta(caminho_arquivo)

    dados_completos = {
        "dados": dados_dict,
        "proximo_id": proximo_id
    }
    
    with open(caminho_arquivo, 'w') as f:
        json.dump(dados_completos, f, indent=4)
