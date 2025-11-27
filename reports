import services
import json
from datetime import datetime

def _imprimir_separador(caractere='-', tamanho=80):
    print(caractere * tamanho)

def relatorio_tarefas_por_projeto():
    _imprimir_separador(tamanho=40)
    print("📋 Relatório de Tarefas por Projeto")
    _imprimir_separador(tamanho=40)
    
    projetos = services.obter_todos('projetos')
    tarefas = services.obter_todos('tarefas')
    usuarios = services.obter_todos('usuarios')

    relatorio = {}
    for proj_id, proj_dados in projetos.items():
        relatorio[proj_id] = {
            'nome': proj_dados.get('nome', 'N/A'),
            'tarefas': []
        }

    for tarefa_id, tarefa_dados in tarefas.items():
        proj_id = tarefa_dados.get('projeto_id')
        
        resp_id = tarefa_dados.get('responsavel_id')
        responsavel = services.buscar_id('usuarios', resp_id)
        nome_responsavel = responsavel.get('Nome', 'Desconhecido') if responsavel else 'Desconhecido'
        
        tarefa_info = {
            'id': tarefa_id,
            'titulo': tarefa_dados.get('titulo', 'Sem Título'),
            'status': tarefa_dados.get('status', 'pendente').title(),
            'prazo': tarefa_dados.get('prazo', 'N/A'),
            'responsavel': nome_responsavel
        }
        
        if proj_id in relatorio:
            relatorio[proj_id]['tarefas'].append(tarefa_info)
    if not relatorio:
        print("Nenhum projeto cadastrado.")
        return

    for proj_id, dados_proj in relatorio.items():
        _imprimir_separador('=')
        print(f"PROJETO: {dados_proj['nome']} (ID: {proj_id})")
        _imprimir_separador('=')
        
        if not dados_proj['tarefas']:
            print("Não há tarefas associadas a este projeto.")
            continue
            
        print(f"{'ID':<8} | {'Título':<40} | {'Status':<12} | {'Prazo':<10} | {'Responsável':<15}")
        _imprimir_separador('-')
        
        for tarefa in dados_proj['tarefas']:
            print(f"{tarefa['id']:<8} | {tarefa['titulo']:<40} | {tarefa['status']:<12} | {tarefa['prazo']:<10} | {tarefa['responsavel']:<15}")
            
    _imprimir_separador('=')

def relatorio_status_geral():
    """Gera um resumo do status de todas as tarefas e projetos."""
    
    _imprimir_separador(tamanho=40)
    print("📊 Status Geral do Sistema")
    _imprimir_separador(tamanho=40)

    # Contagem de Tarefas
    tarefas = services.obter_todos('tarefas')
    total_tarefas = len(tarefas)
    status_contagem = {'pendente': 0, 'andamento': 0, 'concluída': 0}
    
    for _, tarefa_dados in tarefas.items():
        status = tarefa_dados.get('status', 'pendente').lower()
        if status in status_contagem:
            status_contagem[status] += 1
            
    # Contagem de Projetos
    projetos = services.obter_todos('projetos')
    total_projetos = len(projetos)
    
    # Contagem de Usuários
    usuarios = services.obter_todos('usuarios')
    total_usuarios = len(usuarios)

    print(f"Total de Usuários Cadastrados: {total_usuarios}")
    print(f"Total de Projetos Cadastrados: {total_projetos}")
    
    print("\nResumo das Tarefas:")
    print(f"  Total de Tarefas: {total_tarefas}")
    
    if total_tarefas > 0:
        for status, count in status_contagem.items():
            percentual = (count / total_tarefas) * 100
            print(f"  - {status.title():<10}: {count} ({percentual:.1f}%)")
    
    _imprimir_separador(tamanho=40)
    
def relatorio_tarefas_atrasadas():
    """Lista todas as tarefas que estão pendentes ou em andamento e cujo prazo já passou."""
    
    _imprimir_separador(tamanho=40)
    print("⚠️ Relatório de Tarefas Atrasadas")
    _imprimir_separador(tamanho=40)
    
    hoje = datetime.now().strftime('%Y-%m-%d')
    tarefas = services.obter_todos('tarefas')
    atrasadas = []
    
    for tarefa_id, tarefa_dados in tarefas.items():
        status = tarefa_dados.get('status', 'pendente').lower()
        prazo = tarefa_dados.get('prazo')
        
        if status != 'concluída' and prazo:
            if prazo < hoje:
                proj = services.buscar_id('projetos', tarefa_dados.get('projeto_id'))
                nome_projeto = proj.get('nome', 'N/A') if proj else 'N/A'
                resp = services.buscar_id('usuarios', tarefa_dados.get('responsavel_id'))
                nome_responsavel = resp.get('Nome', 'Desconhecido') if resp else 'Desconhecido'

                atrasadas.append({
                    'id': tarefa_id,
                    'titulo': tarefa_dados.get('titulo', 'Sem Título'),
                    'prazo': prazo,
                    'projeto': nome_projeto,
                    'responsavel': nome_responsavel
                })
                
    if not atrasadas:
        print("🎉 Nenhuma tarefa pendente ou em andamento está atrasada. Ótimo trabalho!")
        return

    print(f"{'ID':<8} | {'Título':<40} | {'Prazo':<10} | {'Projeto':<15} | {'Responsável':<15}")
    _imprimir_separador('-')
    
    for tarefa in atrasadas:
        print(f"{tarefa['id']:<8} | {tarefa['titulo']:<40} | {tarefa['prazo']:<10} | {tarefa['projeto']:<15} | {tarefa['responsavel']:<15}")
        
    _imprimir_separador(tamanho=40)

