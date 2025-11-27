import services
import utils
import reports
import sys

def Menu_Principal():
    while True:
        print("""=== Gerenciador de projetos ===
[1] Usuários
[2] Projetos
[3] Tarefas
[4] Relatórios
[0] Sair""")
        
        x=utils.Tratamento()


        if x == 1:
            Menu_Usuario()
        elif x == 2:
            Menu_Projeto()
        elif x == 3:
            Menu_Tarefas()
        elif x == 4:
            Menu_Relatorios()
        elif x == 0:
            print("Saindo...")
            break
        else:
            print("Opção não encontrada. Tente novamente.")
        continue
def Menu_Usuario():
    while True:
        print("""=== Usuário ===
[1] Cadastrar
[2] Listar
[3] Buscar Nome/E-mail
[4] Atualizar
[5] Remover
[6] Voltar para o Menu principal""")
        
        x = utils.Tratamento()
        
        if x == 6:
           break
           
        if x == 1:
            while True:
                while True:
                    nome = input("Insira seu nome: ").strip().title()
                    if len(nome) >= 2: break
                    print("Nome muito curto. Tente novamente.")
                
                while True:
                    email = input("Insira seu E-mail: ").lower()
                    if utils.validar_formato_email(email):
                        break
                    print("Formato de e-mail inválido. Tente novamente (ex: usuario@dominio.com).")
                
                perfil = input("Insira seu perfil (ex: ADM, INFO): ").upper()
                
                dados_para_salvar = {
                    "Nome": nome,
                    "E-mail": email,
                    "Perfil": perfil
                }
                
                id_formatado, mensagem = services.criar('usuarios', dados_para_salvar)
                
                if id_formatado:
                    print(f"Usuário '{nome}' cadastrado com sucesso! ID: {id_formatado}")
                    break
                else:
                    print(f"Falha ao cadastrar: {mensagem}")
                    p = input('Deseja tentar novamente? Digite "Sim" para continuar, "Sair" para voltar ao menu: ')
                    if utils.simsair(p) == '2': break
        
        elif x == 2:
            usuarios_atuais = services.obter_todos('usuarios')
            if not usuarios_atuais:
                print("Nenhum usuário cadastrado.")
            else:
                print("\nLista de Usuários:")
                for user_id, dados in usuarios_atuais.items():
                    print(f"  **ID**: {user_id}, **Nome**: {dados['Nome']}, **E-mail**: {dados['E-mail']}, **Perfil**: {dados['Perfil']}")
            input("Digite qualquer coisa para voltar ao menu: ")

        elif x == 3:
            while True:
                termo = input("Buscar por ID, Nome ou E-mail. Digite o termo: ").strip()
                usuario_encontrado = services.buscar_usuario(termo)
                
                if usuario_encontrado:
                    print('\n--- Usuário Encontrado ---')
                    print(f'ID: {usuario_encontrado["id"]}')
                    print(f'Nome: {usuario_encontrado["Nome"]}')
                    print(f'E-mail: {usuario_encontrado["E-mail"]}')
                    print(f'Perfil: {usuario_encontrado["Perfil"]}')
                    print('--------------------------')
                    input("Digite qualquer coisa para voltar ao menu: ")
                    break
                else:
                    p = input('Usuário não encontrado. Deseja continuar pesquisando? (Sim/Sair): ')
                    if utils.simsair(p) == '2': break

        elif x == 4:
            while True:
                user_id = input("Digite o ID do usuário que deseja atualizar (ex: u_001), ou digite Sair: ").strip().lower()
                if user_id == 'sair': break
                
                usuario_antigo = services.buscar_id('usuarios', user_id)
                if not usuario_antigo:
                    print("Usuário não encontrado pelo ID.")
                    continue
                
                print(f"\nAtualizando usuário ID: {user_id} (Nome atual: {usuario_antigo.get('Nome')})")
                
                while True:
                    novo_nome = input(f"Novo nome (atual: {usuario_antigo['Nome']}): ").strip().title()
                    if len(novo_nome) >= 2: break
                    print("Nome muito curto. Tente novamente.")
                    
                while True:
                    novo_email = input(f"Novo E-mail (atual: {usuario_antigo['E-mail']}): ").lower()
                    if utils.validar_formato_email(novo_email):
                        break
                    print("Formato de e-mail inválido. Tente novamente.")
                    
                novo_perfil = input(f"Novo Perfil (atual: {usuario_antigo['Perfil']}): ").upper()

                dados_salvar = {
                    "Nome": novo_nome,
                    "E-mail": novo_email,
                    "Perfil": novo_perfil
                }
                
                sucesso, mensagem = services.atualizar('usuarios', user_id, dados_salvar)
                
                if sucesso:
                    print(f"Usuário {user_id} atualizado com sucesso!")
                    break
                else:
                    print(f"Falha ao atualizar: {mensagem}")
                    continue

        elif x == 5:
            while True:
                user_id = input("Escreva o ID do usuário que deseja remover (ex: u_001). Digite Sair para sair: ").strip().lower()
                if user_id == 'sair': break
                
                sucesso, mensagem = services.remover_por_id('usuarios', user_id) 
                
                if sucesso:
                    print(f"Usuário {user_id} removido com sucesso!")
                    break 
                else:
                    p = input(f'{mensagem}. Deseja continuar pesquisando? (Sim/Sair): ')
                    if utils.simsair(p) == '2': break


        
        
def Menu_Projeto():
    while True:
        print('''==== Projetos ====
[1] Cadastrar Projeto
[2] Listar Projetos
[3] Buscar Projetos
[4] Atualizar Projetos
[5] Remover Projetos
[0] Voltar''')
        print("Digite o número da opção que deseja selecionar: ")
        x = utils.verifyTXT()

        if (x==0):
            print("Voltando...")
            return
        
        if (x==1):
            while True:
                while True:
                    nome = input("Escreva o nome do seu projeto (NOME ÚNICO): ").strip()
                    if not nome:
                        continue
                    if services.validar_nome_projeto(nome):
                        break
                    else:
                        print("Esse nome de projeto já existe. Tente novamente.")
                        continue            
            
                desc = input("Digite a descrição do seu projeto: ")
                data = utils.obterdatas()
                dados_para_salvar = {
                "nome": nome,
                "nome_upper": nome.upper(),
                "descrição": desc,
                "início": data[0],
                "fim": data[1]
                }
            
                id_formatado, mensagem = services.criar('projetos', dados_para_salvar)
            
                if id_formatado:
                    print(f"Projeto '{nome}' cadastrado com sucesso! ID: {id_formatado}")
                    break
                else:
                    print(f"Falha ao cadastrar: {mensagem}")
                    continue

    
        if (x==2):
            projetos_atuais = services.obter_todos('projetos')
            if not projetos_atuais:
                print("Nenhum projeto cadastrado.")
            else:
                print("\nLista de Projetos:")
                for projeto_id, dados in projetos_atuais.items():
                    print(f"  **ID**: {projeto_id}, **Nome**: {dados['nome']}, **Descrição**: {dados['descrição']}, **Início**: {dados['início']}, **Fim**: {dados['fim'] or 'N/A'}")
            y = input("Digite qualquer coisa para voltar ao menu: ")
            continue

        if (x==3):
            while True:
                z = input("Escreva o ID (ex: p_001) ou o nome do projeto que deseja buscar: ").strip()
                projeto_encontrado = services.buscar_projeto(z)
            
                if projeto_encontrado:
                    print(f'\n--- Projeto Encontrado (ID: {projeto_encontrado["id"]}) ---')
                    print(f'Nome: {projeto_encontrado["nome"]}')
                    print(f'Descrição: {projeto_encontrado["descrição"]}')
                    print(f'Início: {projeto_encontrado["início"]}')
                    print(f'Fim: {projeto_encontrado["fim"] or "Não definido"}')
                    print('-------------------------------------------')
                    input("Digite qualquer coisa para voltar ao menu: ")
                    break
                else:
                    p = input('Projeto não encontrado. Deseja continuar pesquisando? Digite "Sim" para continuar pesquisando.\nSe quiser sair, digite "Sair": ')
                    if utils.simsair(p) == '2':
                        break
        while(x==4):
            a = input("Digite o ID do projeto que deseja atualizar, ou digite Sair para voltar ao menu: ").strip().lower()
            if a == 'sair':
                break
            projeto_antigo=services.buscar_id('projetos', a)
            if not projeto_antigo:
                print("Seu projeto não foi encontrado pelo ID.")
                continue

            projeto_id_atualizar = a
            print(f"\nAtualizando projeto ID: {projeto_id_atualizar} (Nome atual: {projeto_antigo.get('nome')})")
            while True:
                novonome = input("Digite o novo nome do projeto: ").strip()
                if not novonome:
                    continue
                if services.validar_nome_projeto(novonome):
                    break
                else:
                    print("Esse nome já existe. Tente novamente.")
                    continue
            
            desc = input("Digite a descrição do projeto: ")
            data = utils.obterdatas()
            dados_salvar = {
                "nome": novonome,
                "nome_upper": novonome.upper(),
                "descrição": desc,
                "início": data[0],
                "fim": data[1]
            }
            sucesso, mensagem = services.atualizar('projetos', projeto_id_atualizar, dados_salvar)
            if sucesso:
                print(f"Projeto {projeto_id_atualizar} atualizado!")
                break
            else:
                print(f"Falha ao atualizar: {mensagem}")
                continue
        if (x==5):
            while True:
                b = input("Escreva o ID do projeto que deseja remover (ex: p_001). Digite Sair para sair: ").strip().lower()
                if b == 'sair':
                    break
                
                sucesso, mensagem = services.remover_por_id('projetos', b) 

                if sucesso:
                    print(f"Projeto {b} removido com sucesso!")
                    break 
                else:
                    p = input(f'{mensagem}. Deseja continuar pesquisando? Digite "Sim" para continuar pesquisando. Se quiser sair, digite "Sair": ')
                    if utils.simsair(p) == '2':
                        break
  
    


def Menu_Tarefas():
    while True:
        print("""=== Tarefas ===")
[1] Cadastrar
[2] Listar
[3] Atualizar
[4] Remover
[5] Concluir tarefa
[6] Reabrir tarefa
[7] Voltar para o Menu principal""")
        x=utils.Tratamento()

        if x == 7:
            break

        elif x == 1:
            while True:
                titulo = input("Título: ").strip()
                projeto_id = input("ID do projeto (ex: p_001): ").strip().lower()
                responsavel_id = input ("ID do responsável (ex: u_001): ").strip().lower()
                status = input("Status (pendente/andamento/concluída): ").strip().lower()
                prazo = input("Prazo (YYYY-MM-DD): ").strip()
                dados_para_salvar = {
                    "titulo": titulo,
                    "projeto_id": projeto_id,
                    "responsavel_id": responsavel_id,
                    "status": status,
                    "prazo": prazo
                    }
                try:
                    id_formatado, mensagem = services.criar('tarefas', dados_para_salvar)
                    if id_formatado:
                        print(f"Tarefa '{titulo}' cadastrada com sucesso! ID: {id_formatado}")
                        break
                    else:
                        print(f"Falha ao cadastrar: {mensagem}")
                except ValueError as e:
                    print(f"Erro de validação: {e}")
                p = input('Deseja tentar novamente? Digite "Sim" para continuar, "Sair para voltar ao menu: ')
                if utils.simsair(p) == '2': break

        elif x == 2:
            usuarios_atuais = services.obter_todos('usuarios')
            if not usuarios_atuais:
                print("Nenhum usuário cadastrado.")
            else:
                print("\nLista de Usuários:")
                for user_id, dados in usuarios_atuais.items():
                    print(f"  **ID**: {user_id}, **Nome**: {dados['Nome']}, **E-mail**: {dados['E-mail']}, **Perfil**: {dados['Perfil']}")
            input("Digite qualquer coisa para voltar ao menu: ")

        elif x == 3:
            while True:
                termo = input("Buscar por ID, Nome ou E-mail. Digite o termo: ").strip()
                usuario_encontrado = services.buscar_usuario(termo)
                
                if usuario_encontrado:
                    print('\n--- Usuário Encontrado ---')
                    print(f'ID: {usuario_encontrado["id"]}')
                    print(f'Nome: {usuario_encontrado["Nome"]}')
                    print(f'E-mail: {usuario_encontrado["E-mail"]}')
                    print(f'Perfil: {usuario_encontrado["Perfil"]}')
                    print('--------------------------')
                    input("Digite qualquer coisa para voltar ao menu: ")
                    break
                else:
                    p = input('Usuário não encontrado. Deseja continuar pesquisando? (Sim/Sair): ')
                    if utils.simsair(p) == '2': break

        elif x == 4:
            while True:
                user_id = input("Digite o ID do usuário que deseja atualizar (ex: u_001), ou digite Sair: ").strip().lower()
                if user_id == 'sair': break
                
                usuario_antigo = services.buscar_id('usuarios', user_id)
                if not usuario_antigo:
                    print("Usuário não encontrado pelo ID.")
                    continue
                
                print(f"\nAtualizando usuário ID: {user_id} (Nome atual: {usuario_antigo.get('Nome')})")
                
                while True:
                    novo_nome = input(f"Novo nome (atual: {usuario_antigo['Nome']}): ").strip().title()
                    if len(novo_nome) >= 2: break
                    print("Nome muito curto. Tente novamente.")
                    
                while True:
                    novo_email = input(f"Novo E-mail (atual: {usuario_antigo['E-mail']}): ").lower()
                    if utils.validar_formato_email(novo_email):
                        break
                    print("Formato de e-mail inválido. Tente novamente.")
                    
                novo_perfil = input(f"Novo Perfil (atual: {usuario_antigo['Perfil']}): ").upper()

                dados_salvar = {
                    "Nome": novo_nome,
                    "E-mail": novo_email,
                    "Perfil": novo_perfil
                }
                
                sucesso, mensagem = services.atualizar('usuarios', user_id, dados_salvar)
                
                if sucesso:
                    print(f"Usuário {user_id} atualizado com sucesso!")
                    break
                else:
                    print(f"Falha ao atualizar: {mensagem}")
                    continue

        elif x == 5:
            while True:
                user_id = input("Escreva o ID do usuário que deseja remover (ex: u_001). Digite Sair para sair: ").strip().lower()
                if user_id == 'sair': break
                
                sucesso, mensagem = services.remover_por_id('usuarios', user_id) 
                
                if sucesso:
                    print(f"Usuário {user_id} removido com sucesso!")
                    break 
                else:
                    p = input(f'{mensagem}. Deseja continuar pesquisando? (Sim/Sair): ')
                    if utils.simsair(p) == '2': break
                    
def Menu_Projeto():
    while True:
        print('''==== Projetos ====
[1] Cadastrar Projeto
[2] Listar Projetos
[3] Buscar Projetos
[4] Atualizar Projetos
[5] Remover Projetos
[0] Voltar''')
        print("Digite o número da opção que deseja selecionar: ")
        x = utils.verifyTXT()

        if (x==0):
            print("Voltando...")
            return
        
        if (x==1):
            while True:
                while True:
                    nome = input("Escreva o nome do seu projeto (NOME ÚNICO): ").strip()
                    if not nome:
                        print("O nome do projeto não pode ser vazio.")
                        continue
                    if services.validar_nome_projeto(nome):
                        break
                    else:
                        print("Esse nome de projeto já existe. Tente novamente.")
                        continue            
            
                desc = input("Digite a descrição do seu projeto: ")
                data = utils.obterdatas()
                
                dados_para_salvar = {
                "nome": nome,
                "descrição": desc,
                "início": data[0],
                "fim": data[1]
                }
            
                id_formatado, mensagem = services.criar('projetos', dados_para_salvar)
            
                if id_formatado:
                    print(f"Projeto '{nome}' cadastrado com sucesso! ID: {id_formatado}")
                    break
                else:
                    print(f"Falha ao cadastrar: {mensagem}")
                    continue

    
        if (x==2):
            projetos_atuais = services.obter_todos('projetos')
            if not projetos_atuais:
                print("Nenhum projeto cadastrado.")
            else:
                print("\nLista de Projetos:")
                for projeto_id, dados in projetos_atuais.items():
                    print(f"  **ID**: {projeto_id}, **Nome**: {dados['nome']}, **Descrição**: {dados['descrição']}, **Início**: {dados['início']}, **Fim**: {dados['fim'] or 'N/A'}")
            input("Digite qualquer coisa para voltar ao menu: ")

        if (x==3):
            while True:
                z = input("Escreva o ID (ex: p_001) ou o nome do projeto que deseja buscar: ").strip()
                projeto_encontrado = services.buscar_projeto(z)
            
                if projeto_encontrado:
                    print(f'\n--- Projeto Encontrado (ID: {projeto_encontrado["id"]}) ---')
                    print(f'Nome: {projeto_encontrado["nome"]}')
                    print(f'Descrição: {projeto_encontrado["descrição"]}')
                    print(f'Início: {projeto_encontrado["início"]}')
                    print(f'Fim: {projeto_encontrado["fim"] or "Não definido"}')
                    print('-------------------------------------------')
                    input("Digite qualquer coisa para voltar ao menu: ")
                    break
                else:
                    p = input('Projeto não encontrado. Deseja continuar pesquisando? Digite "Sim" para continuar pesquisando.\nSe quiser sair, digite "Sair": ')
                    if utils.simsair(p) == '2':
                        break
                        
        if (x==4):
            while True:
                a = input("Digite o ID do projeto que deseja atualizar, ou digite Sair para voltar ao menu: ").strip().lower()
                if a == 'sair':
                    break
                projeto_antigo = services.buscar_id('projetos', a)
                if not projeto_antigo:
                    print("Seu projeto não foi encontrado pelo ID.")
                    continue

                projeto_id_atualizar = a
                print(f"\nAtualizando projeto ID: {projeto_id_atualizar} (Nome atual: {projeto_antigo.get('nome')})")
                
                while True:
                    novonome = input("Digite o novo nome do projeto: ").strip()
                    if not novonome:
                        print("O nome do projeto não pode ser vazio.")
                        continue
                    if services.validar_nome_projeto(novonome):
                        break
                    else:
                         print("Esse nome de projeto já existe. Tente novamente.")
                         continue
                
                desc = input("Digite a descrição do projeto: ")
                data = utils.obterdatas()

                dados_salvar = {
                    "nome": novonome,
                    "descrição": desc,
                    "início": data[0],
                    "fim": data[1]
                }
                
                sucesso, mensagem = services.atualizar('projetos', projeto_id_atualizar, dados_salvar)
                if sucesso:
                    print(f"Projeto {projeto_id_atualizar} atualizado!")
                    break
                else:
                    print(f"Falha ao atualizar: {mensagem}")
                    continue
                    
        if (x==5):
            while True:
                b = input("Escreva o ID do projeto que deseja remover (ex: p_001). Digite Sair para sair: ").strip().lower()
                if b == 'sair':
                    break
                
                sucesso, mensagem = services.remover_por_id('projetos', b) 

                if sucesso:
                    print(f"Projeto {b} removido com sucesso!")
                    break 
                else:
                    p = input(f'{mensagem}. Deseja continuar pesquisando? Digite "Sim" para continuar pesquisando. Se quiser sair, digite "Sair": ')
                    if utils.simsair(p) == '2':
                        break
                        
def Menu_Tarefas():
    while True:
        print("""=== Tarefas ===
[1] Cadastrar
[2] Listar
[3] Atualizar
[4] Remover
[5] Concluir tarefa
[6] Reabrir tarefa
[7] Voltar para o Menu principal""")
        x = utils.Tratamento()
        
        if x == 7:
            break
            
        elif x == 1:
            while True:
                titulo = input("Título: ").strip()
                projeto_id = input("ID do projeto (ex: p_001): ").strip().lower()
                responsavel_id = input("ID do responsável (ex: u_001): ").strip().lower()
                status = input("Status (pendente/andamento/concluída): ").strip().lower()
                prazo = input("Prazo (YYYY-MM-DD): ").strip()
                
                dados_para_salvar = {
                    "titulo": titulo,
                    "projeto_id": projeto_id,
                    "responsavel_id": responsavel_id,
                    "status": status,
                    "prazo": prazo
                }
                
                try:
                    id_formatado, mensagem = services.criar('tarefas', dados_para_salvar)
                    if id_formatado:
                        print(f"Tarefa '{titulo}' cadastrada com sucesso! ID: {id_formatado}")
                        break
                    else:
                        print(f"Falha ao cadastrar: {mensagem}")
                        
                except ValueError as e:
                    print(f"Erro de validação: {e}")
                
                p = input('Deseja tentar novamente? Digite "Sim" para continuar, "Sair" para voltar ao menu: ')
                if utils.simsair(p) == '2': break

        elif x == 2:
            tarefas_atuais = services.obter_todos('tarefas')
            if not tarefas_atuais:
                print("Nenhuma tarefa cadastrada.")
            else:
                print("\nLista de Tarefas:")
                for tarefa_id, dados in tarefas_atuais.items():
                    print(f"  **ID**: {tarefa_id}, **Título**: {dados['titulo']}, **Projeto**: {dados['projeto_id']}, **Responsável**: {dados['responsavel_id']}, **Status**: {dados['status']}, **Prazo**: {dados['prazo']}")
            input("Digite qualquer coisa para voltar ao menu: ")

        elif x == 3:
            while True:
                tarefa_id = input("Digite o ID da tarefa que deseja atualizar (ex: t_001), ou Sair: ").strip().lower()
                if tarefa_id == 'sair': break
                
                tarefa_antiga = services.buscar_id('tarefas', tarefa_id)
                if not tarefa_antiga:
                    print("Tarefa não encontrada pelo ID.")
                    continue

                print(f"\nAtualizando tarefa ID: {tarefa_id} (Título atual: {tarefa_antiga.get('titulo')})")
                
                novo_titulo = input(f"Novo Título (atual: {tarefa_antiga['titulo']}): ").strip() or tarefa_antiga['titulo']
                novo_projeto_id = input(f"Novo ID do Projeto (atual: {tarefa_antiga['projeto_id']}): ").strip().lower() or tarefa_antiga['projeto_id']
                novo_responsavel_id = input(f"Novo ID do Responsável (atual: {tarefa_antiga['responsavel_id']}): ").strip().lower() or tarefa_antiga['responsavel_id']
                novo_status = input(f"Novo Status (atual: {tarefa_antiga['status']}): ").strip().lower() or tarefa_antiga['status']
                novo_prazo = input(f"Novo Prazo (YYYY-MM-DD) (atual: {tarefa_antiga['prazo']}): ").strip() or tarefa_antiga['prazo']
                
                dados_salvar = {
                    "titulo": novo_titulo,
                    "projeto_id": novo_projeto_id,
                    "responsavel_id": novo_responsavel_id,
                    "status": novo_status,
                    "prazo": novo_prazo
                }
                
                try:
                    sucesso, mensagem = services.atualizar('tarefas', tarefa_id, dados_salvar)
                    
                    if sucesso:
                        print(f"Tarefa {tarefa_id} atualizada com sucesso!")
                        break
                    else:
                        print(f"Falha ao atualizar: {mensagem}")
                        
                except ValueError as e:
                    print(f"Erro de validação: {e}")
                    continue

        elif x == 4:
            while True:
                tarefa_id = input("Escreva o ID da tarefa que deseja remover (ex: t_001). Digite Sair para sair: ").strip().lower()
                if tarefa_id == 'sair': break
                
                sucesso, mensagem = services.remover_por_id('tarefas', tarefa_id) 
                
                if sucesso:
                    print(f"Tarefa {tarefa_id} removida com sucesso!")
                    break 
                else:
                    p = input(f'{mensagem}. Deseja tentar novamente? (Sim/Sair): ')
                    if utils.simsair(p) == '2': break

        elif x == 5:
            tarefa_id = input("Digite o ID da tarefa para marcar como CONCLUÍDA: ").strip().lower()
            sucesso, mensagem = services.concluir_tarefa(tarefa_id)
            print(mensagem)

        elif x == 6:
            tarefa_id = input("Digite o ID da tarefa para REABRIR (status: pendente): ").strip().lower()
            sucesso, mensagem = services.reabrir_tarefa(tarefa_id)
            print(mensagem)

def Menu_Relatorios():
        while True:
            print("\n" + "="*40)
            print("         MENU DE RELATÓRIOS")
            print("="*40)
            print("1. Status Geral do Sistema")
            print("2. Tarefas Agrupadas por Projeto")
            print("3. Tarefas Atrasadas")
            print("4. Voltar ao Menu Principal")
            print("="*40)


            opcao = utils.Tratamento() 

            if opcao == 1:
                reports.relatorio_status_geral()
            elif opcao == 2:
                reports.relatorio_tarefas_por_projeto()
            elif opcao == 3:
                reports.relatorio_tarefas_atrasadas()
            elif opcao == 4:
                break
            else:
                print("❌ Opção inválida. Por favor, digite um número de 1 a 4.")
            
            if opcao in [1, 2, 3]:
                input("\nPressione Enter para continuar e voltar ao menu de relatórios...")
