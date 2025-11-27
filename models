import storage
import utils

def validar_cadastro_usuario(dados_usuario, id_atual=None):
    usuarios_existentes, _ = storage.carregar_dados('usuarios')

    nome_usuario = dados_usuario.get('Nome')
    email_usuario = dados_usuario.get('E-mail')

    for usuario_id, usuario_dados in usuarios_existentes.items():
        if usuario_dados.get('Nome') == nome_usuario and usuario_id != id_atual:
            return False
        if usuario_dados.get('E-mail') == email_usuario and usuario_id != id_atual:
            return False
    return True

def validar_cadastro_projeto(dados_projeto, id_atual=None):
    projetos_existentes, _ = storage.carregar_dados('projetos')
    
    if 'nome_upper' not in dados_projeto:
        return False
    
    nome_projeto_upper = dados_projeto['nome_upper'] 
    
    for projeto_id, projeto_dados in projetos_existentes.items():
        if projeto_dados.get('nome_upper') == nome_projeto_upper and projeto_id != id_atual:
            return False
    return True

def validar_dados_tarefa(dados_tarefa):
    if not dados_tarefa.get('titulo') or dados_tarefa.get('titulo').strip() == "":
        return False, "O título não pode ser vazio."
    if not utils.validar_status(dados_tarefa.get('status')):
        return False, "Status inválido. Use: pendente, andamento, concluída."
    if not utils.validar_data(dados_tarefa.get('prazo')):
        return False, "Prazo inválido. Use o formato YYYY-MM-DD."
    return True, ""

def verificar_regras_especificas(modulo, dados, id_atual=None):
    if modulo == 'projetos':
        return validar_cadastro_projeto(dados, id_atual)
    elif modulo == 'usuarios':
        return validar_cadastro_usuario(dados, id_atual)
    elif modulo == 'tarefas':
        sucesso, mensagem = validar_dados_tarefa(dados)
        if not sucesso:
            raise ValueError(mensagem)
        return sucesso
    return True
