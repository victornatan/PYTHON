
import pyodbc
from tkinter import messagebox
import tkinter as tk
from tkinter import ttk
import re
import os
caminho_localedata = r'C:\Users\victo\PycharmProjects\pythonProject\venv\Lib\site-packages\babel\locale-data'
os.environ['BABEL_DATADIR'] = caminho_localedata
from tkcalendar import DateEntry  # Importa o widget de calendário
from datetime import datetime
import hashlib


# ####################################### CONEXÃO COM O BANCO ##########################################################

def conectar_banco():
    conn_str = 'DRIVER={SQL Server};SERVER=;DATABASE=;UID=;PWD='
    return pyodbc.connect(conn_str)


# ######################################### INSERÇAO DE DADOS ##########################################################

def inserir_cliente(nome, data_de_nascimento, cpf, cnpj, email, rua, numero, bairro, cidade, estado, ddd, telefone):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not nome or not data_de_nascimento or not cpf or not cnpj or not email or not rua or not bairro or not cidade or not estado or not ddd or not telefone:
            messagebox.showinfo('Campos vazios', 'Por favor preencha todos os campos obrigatórios')
            return

        if len(nome.split()) < 2:
            messagebox.showinfo('Nome', 'Por favor, digite o nome completo')
            return

        if not re.match(r'\d{2}\/\d{2}\/\d{4}', data_de_nascimento):
            messagebox.showwarning('Formato DATA', 'O Formato do campo Data de nascimento deve ser: (00/00/0000)')
            return

        if not re.match(r'\d{3}\.\d{3}\.\d{3}\-\d{2}', cpf) or len(cpf) < 11:
            messagebox.showwarning('Formato CPF', 'O Formato do campo CPF deve ser: (000.000.000-00) e conter 11 numeros!')
            return

        if not re.match(r'\d{2}\.\d{3}\.\d{3}\/\d{4}\-\d{2}', cnpj):
            messagebox.showwarning('Formato CNPJ', 'O Formato do CNPJ deve ser: (12.345.678/0003-00) e conter 14 numeros!')
            return

        if not re.match(r'[^@]+@[^@]+\.[^@]+', email):
            messagebox.showwarning('E-MAIL', 'Por favor, Verifique se no campo E-MAIL, contém: ("@", ".")')
            return

        cursor.execute('SELECT CPF, CNPJ FROM CLIENTE WHERE CPF = ? OR CNPJ = ?', cpf, cnpj)
        resultado = cursor.fetchone()
        if resultado:
            messagebox.showwarning('USUÁRIO JÁ CADASTRADO!', 'O USUÁRIO JÁ EXISTE EM NOSSO SISTEMA!')
            return

        else:
            cursor.execute(''' INSERT INTO CLIENTE (DATA, NOME, DATA_DE_NASCIMENTO, CPF, CNPJ, EMAIL, RUA, NUMERO, BAIRRO, CIDADE, ESTADO, DDD, TELEFONE) 
              VALUES (GETDATE(),?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)''', nome, data_de_nascimento, cpf, cnpj, email, rua, numero, bairro, cidade, estado, ddd, telefone)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos')
            limpar_tela()

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível inserir dados {erro}')


def botao_pra_inserir_cliente():

    nome = entry_nome.get()
    data_de_nascimento = entry_data_de_nascimento.get()
    cpf = entry_cpf.get()
    cnpj = entry_cnpj.get()
    email = entry_email.get()
    rua = entry_rua.get()
    numero = entry_numero.get()
    bairro = entry_bairro.get()
    cidade = entry_cidade.get()
    estado = entry_estado.get()
    ddd = entry_ddd.get()
    telefone = entry_telefone.get()

    inserir_cliente(nome, data_de_nascimento, cpf, cnpj, email, rua, numero, bairro, cidade, estado, ddd, telefone)


def inserir_dados_funcionario(data_admissao, nome, cpf, pis, rua, numero, bairro, cidade, estado, ddd, telefone, sexo, idcargo, status, salario):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not data_admissao or not nome or not cpf or not pis or not rua or not bairro or not cidade or not estado or not ddd or not telefone or not idcargo or not status or not salario:
            messagebox.showwarning('Campos Vazios', 'Por favor, Preencha todos os campos obrigatórios')
            return

        if len(nome.split()) < 2:
            messagebox.showinfo('Nome', 'Por favor, digite o nome completo')
            return

        if not re.match(r'\d{2}\/\d{2}\/\d{4}', data_admissao):
            messagebox.showwarning('Formato DATA', 'O Formato do campo Data de Admissão deve ser: (00/00/0000)')
            return

        cursor.execute('SELECT CPF FROM FUNCIONARIO WHERE CPF = ? ', cpf)
        resultado = cursor.fetchone()
        if resultado:
            messagebox.showwarning('USUÁRIO JÁ CADASTRADO!', 'O USUÁRIO JÁ EXISTE EM NOSSO SISTEMA!')
            return

        if not re.match(r'\d{3}\.\d{3}\.\d{3}\-\d{2}', cpf):
            messagebox.showwarning('Formato CPF', 'O Formato do campo CPF deve ser: (000.000.000-00) e conter 11 numeros!')
            return

        if not re.match(r'\d{3}\.\d{5}\.\d{2}\-\d{1}', pis):
            messagebox.showwarning('PIS', 'O Formato do PIS deve ser: (000.00000.00-0) e conter 11 numeros!')
            return

        else:
            cursor.execute('''
                INSERT INTO FUNCIONARIO (DATA_DE_ADMISSAO, NOME, CPF, PIS, RUA, NUMERO, BAIRRO, CIDADE, ESTADO, DDD, TELEFONE, SEXO, ID_CARGO, STATUS, SALARIO_BRUTO, SALARIO_BRUTO_COM_COMISSAO)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, 0)
            ''', data_admissao, nome, cpf, pis, rua, numero, bairro, cidade, estado, ddd, telefone, sexo, idcargo, status, salario)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos')
            limpar_tela_funcionario()
    except pyodbc.Error as erro:
        messagebox.showerror('Erro', 'Não foi possível inserir os dados: ' + str(erro))


# colocando como global os entry para dar pra limpar as telas.
entry_data_admissao = None
entry_nome_func = None
entry_cpf_func = None
entry_pis_func = None
entry_rua_func = None
entry_numero_func = None
entry_bairro_func = None
entry_cidade_func = None
entry_estado_func = None
entry_ddd_func = None
entry_telefone_func = None
entry_sexo_func = None
entry_idcargo_func = None
entry_status_func = None
entry_salario_func = None


# ############################# CRIAÇÃO DAS JANELAS COM OS CAMPOS DE INSERÇÃO ##########################################


def criar_janela_cadastro(): # ao ser chamado ele vai criar outra janela com tudo isso dentro.

    global entry_data_admissao, entry_nome_func, entry_cpf_func, entry_pis_func, entry_rua_func, entry_numero_func, entry_bairro_func, entry_cidade_func, entry_estado_func, entry_ddd_func, entry_telefone_func, entry_sexo_func, entry_idcargo_func, entry_status_func, entry_salario_func

    janelacadastro = tk.Tk()
    janelacadastro.title('Funcionários')
    janelacadastro.geometry('1200x300')

    # Criando os campos para FUNCIONARIO
    label_data_admissao = tk.Label(janelacadastro, text='Data de Admissão: ')
    label_data_admissao.grid(row=0, column=0, padx=10, pady=10)
    entry_data_admissao = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=22)
    entry_data_admissao.grid(row=0, column=1, padx=1, pady=10, sticky="w")
    desc_data_func = tk.Label(janelacadastro, text=f'{"(00/00/0000)"}')
    desc_data_func.grid(row=0, column=1, padx=1, pady=10)

    label_nome_func = tk.Label(janelacadastro, text='Nome: ')
    label_nome_func.grid(row=0, column=2, padx=10, pady=10)
    entry_nome_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=70)
    entry_nome_func.grid(row=0, column=3, padx=10, pady=10)

    label_cpf_func = tk.Label(janelacadastro, text='Cpf: ')
    label_cpf_func.grid(row=0, column=5, padx=10, pady=10)
    entry_cpf_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=40)
    entry_cpf_func.grid(row=0, column=6, padx=10, pady=10, sticky="w")

    label_pis_func = tk.Label(janelacadastro, text='Pis: ')
    label_pis_func.grid(row=1, column=0, padx=10, pady=10)
    entry_pis_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=30)
    entry_pis_func.grid(row=1, column=1, padx=10, pady=10, sticky="w")

    label_rua_func = tk.Label(janelacadastro, text='Rua: ')
    label_rua_func.grid(row=1, column=2, padx=10, pady=10)
    entry_rua_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=70)
    entry_rua_func.grid(row=1, column=3, padx=10, pady=10, sticky="w")

    label_numero_func = tk.Label(janelacadastro, text='Nº: ')
    label_numero_func.grid(row=1, column=5, padx=10, pady=10)
    entry_numero_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=15)
    entry_numero_func.grid(row=1, column=6, padx=10, pady=10, sticky="w")
    desc_salario_func = tk.Label(janelacadastro, text=f'{"(Residência)":<25}')
    desc_salario_func.grid(row=1, column=6, padx=10, pady=10)

    label_bairro_func = tk.Label(janelacadastro, text='Bairro: ')
    label_bairro_func.grid(row=2, column=0, padx=10, pady=10)
    entry_bairro_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=30)
    entry_bairro_func.grid(row=2, column=1, padx=10, pady=10)

    label_cidade_func = tk.Label(janelacadastro, text='Cidade: ')
    label_cidade_func.grid(row=2, column=2, padx=10, pady=10)
    entry_cidade_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=40)
    entry_cidade_func.grid(row=2, column=3, padx=10, pady=10, sticky="w")

    label_estado_func = tk.Label(janelacadastro, text='Estado: ')
    label_estado_func.grid(row=2, column=5, padx=10, pady=10)
    entry_estado_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=15)
    entry_estado_func.grid(row=2, column=6, padx=10, pady=10, sticky="w")

    label_ddd_func = tk.Label(janelacadastro, text='DDD: ')
    label_ddd_func.grid(row=3, column=0, padx=10, pady=10)
    entry_ddd_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=15)
    entry_ddd_func.grid(row=3, column=1, padx=10, pady=10, stick="w")

    label_telefone_func = tk.Label(janelacadastro, text='Telefone: ')
    label_telefone_func.grid(row=3, column=2, padx=10, pady=10)
    entry_telefone_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=30)
    entry_telefone_func.grid(row=3, column=3, padx=10, pady=10, stick="w")

    label_sexo_func = tk.Label(janelacadastro, text='Sexo: ')
    label_sexo_func.grid(row=3, column=5, padx=10, pady=10)
    entry_sexo_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=20)
    entry_sexo_func.grid(row=3, column=6, padx=10, pady=10, stick="w")
    desc_sexo_func = tk.Label(janelacadastro, text=f'{"(M / F)":<25}')
    desc_sexo_func.grid(row=3, column=6, padx=10, pady=10)

    label_idcargo_func = tk.Label(janelacadastro, text='ID Cargo: ')
    label_idcargo_func.grid(row=4, column=0, padx=10, pady=10)
    entry_idcargo_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=20)
    entry_idcargo_func.grid(row=4, column=1, padx=10, pady=10, stick="w")

    label_status_func = tk.Label(janelacadastro, text='Status: ')
    label_status_func.grid(row=4, column=2, padx=10, pady=10)
    entry_status_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=20)
    entry_status_func.grid(row=4, column=3, padx=10, pady=10, stick="w")
    desc_status_func = tk.Label(janelacadastro, text=f'{"(Pendente/Ativo/Desativado)":<30}')
    desc_status_func.grid(row=4, column=3, padx=10, pady=10)

    label_salario_func = tk.Label(janelacadastro, text='Salário: ')
    label_salario_func.grid(row=4, column=5, padx=10, pady=10)
    entry_salario_func = ttk.Entry(janelacadastro, style='EstiloEntry.TEntry', width=25)
    entry_salario_func.grid(row=4, column=6, padx=10, pady=10, stick="w")
    desc_salario_func = tk.Label(janelacadastro, text=f'{"(Separado por Pontos . )":<25}')
    desc_salario_func.grid(row=4, column=6, padx=10, pady=10)

    botao_inserir_funcionario = tk.Button(janelacadastro, text='Inserir Funcionário', bg='blue', fg='black', bd=5, font=('Helvetica', 10, 'bold'), command=lambda: inserir_dados_funcionario(
        entry_data_admissao.get(),
        entry_nome_func.get(),
        entry_cpf_func.get(),
        entry_pis_func.get(),
        entry_rua_func.get(),
        entry_numero_func.get(),
        entry_bairro_func.get(),
        entry_cidade_func.get(),
        entry_estado_func.get(),
        entry_ddd_func.get(),
        entry_telefone_func.get(),
        entry_sexo_func.get(),
        entry_idcargo_func.get(),
        entry_status_func.get(),
        entry_salario_func.get()
        ))
    botao_inserir_funcionario.grid(row=6, column=3, padx=10, pady=10)


# Criaçao da janela de cadastrar cargos
def criar_janela_cadastro_cargo():

    global entry_nome_cargo

    janelacargo = tk.Tk()
    janelacargo.title('Cadastro de Cargos')
    janelacargo.geometry('260x150')

    label_nome_cargo = tk.Label(janelacargo, text='Nome Cargo:')
    label_nome_cargo.grid(row=0, column=0, padx=10, pady=10)
    entry_nome_cargo = ttk.Entry(janelacargo, style='EstiloEntry.TEntry', width=20)
    entry_nome_cargo.grid(row=0, column=1, padx=10, pady=10)

    botao_inserir_cargo = tk.Button(janelacargo, text='Inserir Cargo', bg='blue', fg='white', bd=5, font=('Helvetica', 8, 'bold'), command=lambda: cadastrar_cargo(entry_nome_cargo.get()))
    botao_inserir_cargo.grid(row=1, column=1, padx=10, pady=10)


entry_estado1 = None


def criar_janela_cadastro_estado():

    global entry_estado1

    janelaestado = tk.Tk()
    janelaestado.title('Estados')
    janelaestado.geometry('250x120')

    label_estado1 = tk.Label(janelaestado, text='Estado:')
    label_estado1.grid(row=0, column=0, padx=10, pady=10)
    entry_estado1 = ttk.Entry(janelaestado, style='EstiloEntry.TEntry', width=20)
    entry_estado1.grid(row=0, column=1, padx=10, pady=10)

    botao_inserir_estado = tk.Button(janelaestado, text='Inserir Estado', bg='blue', fg='white', takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: inserir_dados_estado(entry_estado1.get()))
    botao_inserir_estado.grid(row=1, column=1, padx=10, pady=10)


entry_idfuncionario = None
entry_idestado = None


def criar_janela_cadastro_area():

    global entry_idfuncionario, entry_idestado

    janelaarea = tk.Tk()
    janelaarea.title('Cadastro de Área')
    janelaarea.geometry('350x150')

    label_idfuncionario = tk.Label(janelaarea, text='ID FUNCIONÁRIO:')
    label_idfuncionario.grid(row=0, column=0, padx=10, pady=10,)
    entry_idfuncionario = ttk.Entry(janelaarea,  style='EstiloEntry.TEntry', width=20)
    entry_idfuncionario.grid(row=0, column=1, padx=10, pady=10)

    label_idestado = tk.Label(janelaarea, text='ID ESTADO:')
    label_idestado.grid(row=1, column=0, padx=10, pady=10)
    entry_idestado = ttk.Entry(janelaarea, style='EstiloEntry.TEntry', width=20)
    entry_idestado.grid(row=1, column=1, padx=10, pady=10)

    botao_inserir_area = tk.Button(janelaarea, text='Inserir Área', bg='blue', fg='white', takefocus=False, bd=3, command=lambda: cadastrar_area_atuacao(entry_idfuncionario.get(), entry_idestado.get()))
    botao_inserir_area.grid(row=2, column=1, padx=10, pady=10)


entry_idfuncionariom = None
entry_valor_meta = None
entry_data_final_meta = None


def criar_janela_cadastro_meta():

    global entry_idfuncionariom, entry_valor_meta, entry_data_final_meta

    janelameta = tk.Tk()
    janelameta.title('Cadastro de Meta')
    janelameta.geometry('400x230')

    label_idfuncionario = tk.Label(janelameta, text='ID VENDEDOR:')
    label_idfuncionario.grid(row=0, column=1, padx=10, pady=10)
    entry_idfuncionariom = ttk.Entry(janelameta,style='EstiloEntry.TEntry', width=20)
    entry_idfuncionariom.grid(row=0, column=2, padx=10, pady=10)

    label_valor_meta = tk.Label(janelameta, text='Valor Meta:')
    label_valor_meta.grid(row=1, column=1, padx=10, pady=10)
    entry_valor_meta = ttk.Entry(janelameta, style='EstiloEntry.TEntry', width=20)
    entry_valor_meta.grid(row=1, column=2, padx=10, pady=10)

    label_data_final_meta = tk.Label(janelameta, text='Data Final Da Meta:')
    label_data_final_meta.grid(row=2, column=1, padx=10, pady=10)
    entry_data_final_meta = ttk.Entry(janelameta, style='EstiloEntry.TEntry', width=20)
    entry_data_final_meta.grid(row=2, column=2, padx=10, pady=10)

    botao_inserir_meta = tk.Button(janelameta, text='Inserir Meta', bg='blue', fg='white', bd=5, font=('Helvetica', 8, 'bold'), command=lambda: cadastrar_meta(entry_idfuncionariom.get(), entry_valor_meta.get(), entry_data_final_meta.get()))
    botao_inserir_meta.grid(row=3, column=2, padx=10, pady=10)


entry_nomep = None
entry_tipo = None
entry_cor = None
entry_turbo = None
entry_porta = None
entry_preco = None


def criar_janela_cadastro_produto():

    global entry_nomep, entry_tipo, entry_cor, entry_turbo, entry_porta, entry_preco

    janelaproduto = tk.Tk()
    janelaproduto.title('Cadastro de Produto')
    janelaproduto.geometry('400x300')

    label_nomep = tk.Label(janelaproduto, text='Nome Produto:')
    label_nomep.grid(row=0, column=1, padx=10, pady=10)
    entry_nomep = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_nomep.grid(row=0, column=2, padx=10, pady=10)

    label_tipo = tk.Label(janelaproduto, text='Descrição:')
    label_tipo.grid(row=1, column=1, padx=10, pady=10)
    entry_tipo = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_tipo.grid(row=1, column=2, padx=10, pady=10)

    label_cor = tk.Label(janelaproduto, text='Cor:')
    label_cor.grid(row=2, column=1, padx=10, pady=10)
    entry_cor = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_cor.grid(row=2, column=2, padx=10, pady=10)

    label_turbo = tk.Label(janelaproduto, text='Turbo:')
    label_turbo.grid(row=3, column=1, padx=10, pady=10)
    entry_turbo = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_turbo.grid(row=3, column=2, padx=10, pady=10)

    label_porta = tk.Label(janelaproduto, text='Portas:')
    label_porta.grid(row=4, column=1, padx=10, pady=10)
    entry_porta = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_porta.grid(row=4, column=2, padx=10, pady=10)

    label_preco = tk.Label(janelaproduto, text='Preço:')
    label_preco.grid(row=5, column=1, padx=10, pady=10)
    entry_preco = ttk.Entry(janelaproduto, style='EstiloEntry.TEntry', width=20)
    entry_preco.grid(row=5, column=2, padx=10, pady=10)

    botao_inserir_meta = tk.Button(janelaproduto, text='Inserir Meta', bg='blue', fg='white', bd=5, font=('Helvetica', 8, 'bold'), command=lambda: cadastrar_produto(entry_nomep.get(), entry_tipo.get(), entry_cor.get(), entry_turbo.get(), entry_porta.get(), entry_preco.get()))
    botao_inserir_meta.grid(row=6, column=2, padx=10, pady=10)


entry_nomef = entry_tipomaterial = entry_cnpjf = entry_dddf = entry_telefonef = None
entry_emailf = entry_ruaf = entry_nf = entry_bairrof = entry_cidadef = None
entry_estadof = entry_situacaof = None


def criar_janela_cadastro_fornecedor():
    global entry_nomef, entry_tipomaterial, entry_cnpjf, entry_dddf, entry_telefonef
    global entry_emailf, entry_ruaf, entry_nf, entry_bairrof, entry_cidadef
    global entry_estadof, entry_situacaof

    janelafornecedor = tk.Tk()
    janelafornecedor.title('Fornecedores')
    janelafornecedor.geometry('1200x300')

    label_nomef = tk.Label(janelafornecedor, text='Nome da Empresa:')
    label_nomef.grid(row=0, column=0, padx=10, pady=10)
    entry_nomef = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_nomef.grid(row=0, column=1, padx=10, pady=10)

    label_tipomaterial = tk.Label(janelafornecedor, text='Tipo de Material:')
    label_tipomaterial.grid(row=0, column=2, padx=10, pady=10)
    entry_tipomaterial = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=50)
    entry_tipomaterial.grid(row=0, column=3, padx=10, pady=10)

    label_cnpjf = tk.Label(janelafornecedor, text='Cnpj:')
    label_cnpjf.grid(row=0, column=4, padx=10, pady=10)
    entry_cnpjf = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_cnpjf.grid(row=0, column=5, padx=10, pady=10)

    label_dddf = tk.Label(janelafornecedor, text='DDD:')
    label_dddf.grid(row=1, column=0, padx=10, pady=10)
    entry_dddf = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=10)
    entry_dddf.grid(row=1, column=1, padx=10, pady=10, sticky="w")

    label_telefonef = tk.Label(janelafornecedor, text='Telefone:')
    label_telefonef.grid(row=1, column=2, padx=10, pady=10)
    entry_telefonef = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_telefonef.grid(row=1, column=3, padx=10, pady=10,  sticky="w")

    label_emailf = tk.Label(janelafornecedor, text='E-MAIL:')
    label_emailf.grid(row=1, column=4, padx=10, pady=10)
    entry_emailf = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_emailf.grid(row=1, column=5, padx=10, pady=10)

    label_ruaf = tk.Label(janelafornecedor, text='Rua:')
    label_ruaf.grid(row=2, column=0, padx=10, pady=10)
    entry_ruaf = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_ruaf.grid(row=2, column=1, padx=10, pady=10)

    label_nf = tk.Label(janelafornecedor, text='Número:')
    label_nf.grid(row=2, column=2, padx=10, pady=10)
    entry_nf = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=15)
    entry_nf.grid(row=2, column=3, padx=10, pady=10, sticky="w")
    desc_nf = tk.Label(janelafornecedor, text='(Opcional)')
    desc_nf.grid(row=2, column=3)

    label_bairrof = tk.Label(janelafornecedor, text='Bairro:')
    label_bairrof.grid(row=2, column=4, padx=10, pady=10)
    entry_bairrof = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_bairrof.grid(row=2, column=5, padx=10, pady=10, sticky="w")

    label_cidadef = tk.Label(janelafornecedor, text='Cidade:')
    label_cidadef.grid(row=3, column=0, padx=10, pady=10)
    entry_cidadef = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_cidadef.grid(row=3, column=1, padx=10, pady=10)

    label_estadof = tk.Label(janelafornecedor, text='Estado:')
    label_estadof.grid(row=3, column=2, padx=10, pady=10)
    entry_estadof = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_estadof.grid(row=3, column=3, padx=10, pady=10, sticky="w")

    label_situacaof = tk.Label(janelafornecedor, text='Situação:')
    label_situacaof.grid(row=3, column=4, padx=10, pady=10)
    entry_situacaof = ttk.Entry(janelafornecedor, style='EstiloEntry.TEntry', width=40)
    entry_situacaof.grid(row=3, column=5, padx=10, pady=10)

    botao_inserir_fornecedor = tk.Button(janelafornecedor, text='Inserir Forncedor', bg='blue', fg='white', bd=5, font=('Helvetica', 8, 'bold'), command=lambda: inserir_dados_fornecedor(entry_nomef.get(), entry_tipomaterial.get(), entry_cnpjf.get(), entry_dddf.get(), entry_telefonef.get(), entry_emailf.get(), entry_ruaf.get(), entry_nf.get(), entry_bairrof.get(), entry_cidadef.get(), entry_estadof.get(), entry_situacaof.get()))
    botao_inserir_fornecedor.grid(row=4, column=3, padx=10, pady=10)


entry_id_fornecedor = entry_lote = entry_nome_produto = entry_tipo_material = entry_cor_material = None
entry_tamanho = entry_quantidade = entry_data_validade = None


def criar_janela_cadastro_estoque():

    global entry_id_fornecedor, entry_lote, entry_nome_produto, entry_tipo_material, entry_cor_material
    global entry_tamanho, entry_quantidade, entry_data_validade

    janelaestoque = tk.Tk()
    janelaestoque.title('Estoque Material')
    janelaestoque.geometry('600x300')

    label_id_fornecedor = tk.Label(janelaestoque, text='ID FORNECEDOR:')
    label_id_fornecedor.grid(row=0, column=0, padx=10, pady=10)
    entry_id_fornecedor = ttk.Entry(janelaestoque)
    entry_id_fornecedor.grid(row=0, column=1, padx=10, pady=10, sticky="w")

    label_lote = tk.Label(janelaestoque, text='Lote:')
    label_lote.grid(row=0, column=1, padx=10, pady=10)
    entry_lote = ttk.Entry(janelaestoque)
    entry_lote.grid(row=0, column=1, padx=10, pady=10, sticky="e")

    label_nome_produto = tk.Label(janelaestoque, text='Nome Produto:')
    label_nome_produto.grid(row=2, column=0, padx=10, pady=10)
    entry_nome_produto = ttk.Entry(janelaestoque,  width=60)
    entry_nome_produto.grid(row=2, column=1, padx=10, pady=10)

    label_tipo_material = tk.Label(janelaestoque, text='Tipo de Material:')
    label_tipo_material.grid(row=3, column=0, padx=10, pady=10)
    entry_tipo_material = ttk.Entry(janelaestoque)
    entry_tipo_material.grid(row=3, column=1, padx=10, pady=10, sticky="w")

    label_cor_material = tk.Label(janelaestoque, text='Cor:')
    label_cor_material.grid(row=3, column=1, padx=10, pady=10)
    entry_cor_material = ttk.Entry(janelaestoque)
    entry_cor_material.grid(row=3, column=1, padx=10, pady=10, sticky="e")

    label_tamanho = tk.Label(janelaestoque, text='Tamanho:')
    label_tamanho.grid(row=4, column=0, padx=10, pady=10)
    entry_tamanho = ttk.Entry(janelaestoque)
    entry_tamanho.grid(row=4, column=1, padx=10, pady=10, sticky="w")

    label_quantidade = tk.Label(janelaestoque, text='Quantidade:')
    label_quantidade.grid(row=4, column=1, padx=10, pady=10)
    entry_quantidade = ttk.Entry(janelaestoque)
    entry_quantidade.grid(row=4, column=1, padx=10, pady=10, sticky="e")

    label_data_validade = tk.Label(janelaestoque, text='Data de Validade:')
    label_data_validade.grid(row=5, column=0, padx=10, pady=10)
    entry_data_validade = ttk.Entry(janelaestoque)
    entry_data_validade.grid(row=5, column=1, padx=10, pady=10, sticky="w")

    botao_inserir_estoque = tk.Button(janelaestoque, text='Inserir Produto', bg='blue', fg='white', bd=5, font=('Helvetica', 10, 'bold'), command=lambda: inserir_cadastro_estoque(entry_id_fornecedor.get(), entry_lote.get(), entry_nome_produto.get(), entry_tipo_material.get(), entry_cor_material.get(), entry_tamanho.get(), entry_quantidade.get(), entry_data_validade.get()))
    botao_inserir_estoque.grid(row=5, column=1, padx=10, pady=10, sticky="e")


entry_id_cliente = entry_id_produto = entry_id_vendedor = entry_quantidade_p = entry_data_entrega_p = None
entry_modo_pagamento = entry_parcelas = None


def criar_cadastro_pedido():

    global entry_id_cliente, entry_id_produto, entry_id_vendedor, entry_quantidade_p, entry_data_entrega_p
    global entry_modo_pagamento, entry_parcelas

    janelapedido = tk.Tk()
    janelapedido.title('Pedidos')
    janelapedido.geometry('400x400')

    label_id_cliente = tk.Label(janelapedido, text='ID CLIENTE:')
    label_id_cliente.grid(row=0, column=0, padx=10, pady=10)
    entry_id_cliente = ttk.Entry(janelapedido)
    entry_id_cliente.grid(row=0, column=1, padx=10, pady=10)

    label_id_produto = tk.Label(janelapedido, text='ID PRODUTO:')
    label_id_produto.grid(row=1, column=0, padx=10, pady=10)
    entry_id_produto = ttk.Entry(janelapedido)
    entry_id_produto.grid(row=1, column=1, padx=10, pady=0)

    label_id_vendedor = tk.Label(janelapedido, text='ID VENDEDOR:')
    label_id_vendedor.grid(row=2, column=0, padx=10, pady=10)
    entry_id_vendedor = ttk.Entry(janelapedido)
    entry_id_vendedor.grid(row=2, column=1, padx=10, pady=10)

    label_quantidade_p = tk.Label(janelapedido, text='QUANTIDADE:')
    label_quantidade_p.grid(row=3, column=0, padx=10, pady=10)
    entry_quantidade_p = ttk.Entry(janelapedido)
    entry_quantidade_p.grid(row=3, column=1, padx=10, pady=10)

    label_data_entrega_p = tk.Label(janelapedido, text='DATA DA ENTREGA:')
    label_data_entrega_p.grid(row=4, column=0, padx=10, pady=10)
    entry_data_entrega_p = ttk.Entry(janelapedido)
    entry_data_entrega_p.grid(row=4, column=1, padx=10, pady=10)

    label_modo_pagamento = tk.Label(janelapedido, text='Modo de Pagamento:')
    label_modo_pagamento.grid(row=5, column=0, padx=10, pady=10)
    entry_modo_pagamento = ttk.Entry(janelapedido)
    entry_modo_pagamento.grid(row=5, column=1, padx=10, pady=10)

    label_parcelas = tk.Label(janelapedido, text='Parcelas:')
    label_parcelas.grid(row=6, column=0, padx=10, pady=10)
    entry_parcelas = ttk.Entry(janelapedido)
    entry_parcelas.grid(row=6, column=1, padx=10, pady=10)

    botao_inserir_pedido = tk.Button(janelapedido, text='Inserir Pedido', command=lambda: inserir_dados_pedido(entry_id_cliente.get(), entry_id_produto.get(), entry_id_vendedor.get(), entry_quantidade_p.get(), entry_data_entrega_p.get(), entry_modo_pagamento.get(), entry_parcelas.get()), bg='blue', fg='white', bd=5, font=('Helvetica', 10, 'bold'))
    botao_inserir_pedido.grid(row=7, column=1, padx=10, pady=10)


entry_id_pedido = entry_id_estoque = entry_quantidade_consumo = None


def cadastro_saida_material():

    global entry_id_pedido, entry_id_estoque, entry_quantidade_consumo

    janelaconsumo = tk.Tk()
    janelaconsumo.title('Material de Consumo')
    janelaconsumo.geometry('250x250')

    label_id_pedido = tk.Label(janelaconsumo, text='ID PEDIDO:')
    label_id_pedido.grid(row=0, column=0, padx=10, pady=10)
    entry_id_pedido = ttk.Entry(janelaconsumo)
    entry_id_pedido.grid(row=0, column=1, padx=10, pady=10)

    label_id_estoque = tk.Label(janelaconsumo, text='ID ESTOQUE:')
    label_id_estoque.grid(row=1, column=0, padx=10, pady=10)
    entry_id_estoque = ttk.Entry(janelaconsumo)
    entry_id_estoque.grid(row=1, column=1, padx=10, pady=10)

    label_quantidade_consumo = tk.Label(janelaconsumo, text='QUANTIDADE:')
    label_quantidade_consumo.grid(row=2, column=0, padx=10, pady=10)
    entry_quantidade_consumo = ttk.Entry(janelaconsumo)
    entry_quantidade_consumo.grid(row=2, column=1, padx=10, pady=10)

    botao_saida_material = tk.Button(janelaconsumo, text='Retirar Material', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: inserir_dados_retirada(entry_id_pedido.get(), entry_id_estoque.get(), entry_quantidade_consumo.get()))
    botao_saida_material.grid(row=3, column=1, padx=10, pady=10)

    janelaconsumo.mainloop()


entry_id_funcionario = entry_calculo = None


def cadastro_folha():

    global entry_id_funcionario, entry_calculo

    janelacalculo = tk.Tk()
    janelacalculo.title('NF')
    janelacalculo.geometry('300x150')

    label_id_funcionario = tk.Label(janelacalculo, text='ID FUNCIONARIO:')
    label_id_funcionario.grid(row=0, column=0, padx=10, pady=10)
    entry_id_funcionario = ttk.Entry(janelacalculo)
    entry_id_funcionario.grid(row=0, column=1, padx=10, pady=10)

    label_calculo = tk.Label(janelacalculo, text=' % DESCONTO INSS:')
    label_calculo.grid(row=1, column=0, padx=10, pady=10)
    entry_calculo = ttk.Entry(janelacalculo)
    entry_calculo.grid(row=1, column=1, padx=10, pady=10)

    botao_saida_material = tk.Button(janelacalculo, text='Gerar Nota Fiscal', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: inserir_dados_folha(entry_id_funcionario.get(), entry_calculo.get()))
    botao_saida_material.grid(row=2, column=0, padx=10, pady=10)

    janelacalculo.mainloop()


# ################################### CRIAÇÃO DAS JANELAS DE EXIBIÇÃO DOS RESULTADOS ###################################

def criar_janela_exibicao_funcionario(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('800x400')

    def exibir_dados_no_treeview(filtro_cpf=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')
            if filtro_cpf is None or filtro_cpf == valores[3]:  # Índice 3 é o campo CPF
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o CPF
    entry_cpf1 = tk.Entry(janelaexibicao, width=15)
    entry_cpf1.pack(side='top', pady=10)

    # Botão para acionar a filtragem por CPF
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar por CPF', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_cpf1.get()))
    button_filtrar.pack(side='top', padx=10)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM FUNCIONARIO')
    total_funcionario = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de funcionários
    label_total_funcionario = tk.Label(janelaexibicao, text=f'Total de Funcionários: {total_funcionario}', font=('Arial', 12, 'bold'))
    label_total_funcionario.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID FUNC', 'DATA ADMISSÃO', 'NOME', 'CPF', 'PIS', 'RUA', 'Nº', 'BAIRRO', 'CIDADE', 'UF', 'DDD', 'TEL', 'SEXO', 'ID CARGO', 'STATUS', 'SALÁRIO BRUTO', 'SALÁRIO BRUTO COMISSÃO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [5, 30, 160, 65, 70, 100, 5, 45, 30, 5, 5, 43, 5, 5, 30, 50, 60]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    #exibir_dados_no_treeview() vamos retirar para que os dados todos nao venha aparecer tudo na tela, so quero os que eu filtrar
    janelaexibicao.mainloop()


# visualizaçao dos dados do cliente ###
def criar_janela_exibicao_cliente(resultados):

    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('1200x600')

    def filtrar_dados():
        termo = entry_filtro.get().strip().lower()
        dados_filtrados = [resultado for resultado in resultados if termo in resultado.lower()]
        exibir_dados_no_treeview(dados_filtrados)

    def exibir_dados_no_treeview(dados):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        if not dados:
            messagebox.showwarning('Sem Dados', 'Não temos registro pelo campo buscado')
            return
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados
            for resultado in dados:
                treeview.insert('', 'end', values=resultado.split(', '))

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM CLIENTE')
    total_clientes = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de clientes
    label_total_clientes = tk.Label(janelaexibicao, text=f'Total de Clientes: {total_clientes}', font=('Arial', 12, 'bold'))
    label_total_clientes.pack(side='top', padx=10, pady=5)

    # Adiciona campo de filtro
    label_filtro = tk.Label(janelaexibicao, text='Filtrar por CPF/CNPJ:')
    label_filtro.pack(pady=5)

    entry_filtro = tk.Entry(janelaexibicao, width=20)
    entry_filtro.pack(pady=5)

    botao_filtrar = tk.Button(janelaexibicao, text='Filtrar', command=filtrar_dados, bg='blue', fg='white', bd=3)
    botao_filtrar.pack(pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID', 'Nome', 'Data de Nascimento', 'CPF', 'CNPJ', 'E-mail', 'Rua', 'Número', 'Bairro', 'Cidade', 'Estado', 'DDD', 'Telefone')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [40, 80, 80, 90, 90, 90, 80, 40, 70, 80, 50, 30, 80]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    janelaexibicao.mainloop()


def criar_janela_exibicao_cargo(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização de Cargos')
    janelaexibicao.geometry('600x300')

    def exibir_dados_no_treeview(filtro_nome=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')
            if filtro_nome is None or filtro_nome.lower() in valores[1].lower():  # Índice 1 é o campo CARGO
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o nome
    entry_nome_filtro = tk.Entry(janelaexibicao, width=15)
    entry_nome_filtro.pack(side='top', pady=10)

    # Botão para acionar a filtragem por nome
    button_filtrar_nome = tk.Button(janelaexibicao, text='Filtrar por Nome', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_nome_filtro.get()))
    button_filtrar_nome.pack(side='top', padx=10)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM cargo')
    total_cargos = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de clientes
    label_total_cargos = tk.Label(janelaexibicao, text=f'Total de Cargos: {total_cargos}', font=('Arial', 12, 'bold'))
    label_total_cargos.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('IDCARGO', 'CARGO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [80, 80]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()
    janelaexibicao.mainloop()


entry_nome_cargo = None


def criar_janela_exibicao_estado(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('600x300')

    def exibir_dados_no_treeview():
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        for resultado in dados:
            treeview.insert('', 'end', values=resultado.split(', '))

        if not dados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM estado')
    total_estados = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de clientes
    label_total_estados = tk.Label(janelaexibicao, text=f'Total de Estados: {total_estados}', font=('Arial', 12, 'bold'))
    label_total_estados.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('IDESTADO', 'UF')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [80, 80]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_area(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('600x300')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')
            id_vendedor, id_estado = map(int, valores)  # Converte para inteiros para comparação

            if filtro is None or str(filtro) in map(str, [id_vendedor, id_estado]):
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID VENDEDOR/ID ESTADO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM AREA_DE_ATUACAO_VENDEDOR')
    total_area = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_area = tk.Label(janelaexibicao, text=f'Total de Áreas: {total_area}', font=('Arial', 12, 'bold'))
    label_total_area.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID VENDEDOR', 'ID FUNCIONÁRIO', 'ID ESTADO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [60, 60, 60]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_meta(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('800x400')

    def exibir_dados_no_treeview(filtro_id_vendedor=None, filtro_data=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')
            id_vendedor, data = valores[1], valores[4]

            # Converte a data para o formato do banco
            data_banco = datetime.strptime(data, '%Y-%m-%d')

            # Converte a data do filtro para o mesmo formato
            filtro_data_banco = None
            if filtro_data:
                try:
                    # Ajusta o formato da data do calendário
                    filtro_data_banco = datetime.strptime(filtro_data, '%m/%d/%y')
                except ValueError:
                    # Tratamento de erro caso a conversão falhe
                    label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
                    return

            # Verifica se o resultado atende ao filtro de ID do vendedor
            filtro_id_vendedor = filtro_id_vendedor.lower() if filtro_id_vendedor else None
            id_vendedor_lower = id_vendedor.lower()
            if (filtro_id_vendedor is None or filtro_id_vendedor in id_vendedor_lower) and \
                    (filtro_data_banco is None or filtro_data_banco.date() == data_banco.date()):
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Rótulo para indicar o campo de filtro por ID do vendedor
    label_id_vendedor = tk.Label(janelaexibicao, text='Filtrar Por ID do Vendedor:')
    label_id_vendedor.pack(side='top', pady=5)

    # Caixa de entrada para o filtro por ID do vendedor
    entry_filtro_id_vendedor = tk.Entry(janelaexibicao, width=10)
    entry_filtro_id_vendedor.pack(side='top', pady=5)

    # Caixa de entrada para o filtro por data (calendário)
    calendar_label = tk.Label(janelaexibicao, text='Filtrar Por Data:')
    calendar_label.pack(side='top', pady=5)

    # Widget de calendário
    entry_filtro_data = DateEntry(janelaexibicao, width=12, background='darkblue', foreground='white', borderwidth=2, date_pattern='mm/dd/yy')
    entry_filtro_data.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro_id_vendedor.get(), entry_filtro_data.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM META_VENDEDOR')
    total_meta = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de clientes
    label_total_meta = tk.Label(janelaexibicao, text=f'Total de Metas: {total_meta}', font=('Arial', 12, 'bold'))
    label_total_meta.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID META', 'ID VENDEDOR', 'VALOR META', 'ANDAMENTO', 'DATA FINAL META')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [80, 80, 80, 80, 80]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_produto(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Visualização')
    janelaexibicao.geometry('600x300')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')

            if filtro is None or str(filtro) == valores[0]: # PARA  PESQUISA EXATA. SE EU QUISER PARCIALMENTE in valores:
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PRODUTO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM PRODUTO')
    total_produto = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_produto = tk.Label(janelaexibicao, text=f'Total de Produtos: {total_produto}', font=('Arial', 12, 'bold'))
    label_total_produto.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID PRODUTO', 'NOME', 'DESCRIÇÃO', 'COR', 'TURBO', 'PORTAS', 'PREÇO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [20, 60, 250, 20, 30, 30, 40]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_fornecedor(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Forncedores')
    janelaexibicao.geometry('600x300')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')

            if filtro is None or str(filtro) == valores[4]: # PARA  PESQUISA EXATA. SE EU QUISER PARCIALMENTE in valores: valores[4] é o indice da coluna que quero filtrar (cnpj)
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (CNPJ)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM FORNECEDOR')
    total_fornecedor = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_fornecedor = tk.Label(janelaexibicao, text=f'Total de Fornecedor: {total_fornecedor}', font=('Arial', 12, 'bold'))
    label_total_fornecedor.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID FORNECEDOR', 'DATA DE ENTRADA', 'NOME', 'TIPO DE MATERIAL', 'CNPJ', 'DDD', 'TELEFONE', 'E-MAIL', 'NUMERO', 'RUA', 'BAIRRO', 'CIDADE', 'ESTADO', 'SITUAÇÃO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [20, 60, 30, 20, 30, 30, 40, 40, 30, 30, 40, 40, 30, 40]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_estoque(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Estoque de Material')
    janelaexibicao.geometry('1200x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')

            if filtro is None or str(filtro) == valores[0]: # PARA  PESQUISA EXATA. SE EU QUISER PARCIALMENTE in valores:
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PRODUTO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM ESTOQUE_DE_MATERIAL')
    total_estoque = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_estoque = tk.Label(janelaexibicao, text=f'Total de Material Estoque: {total_estoque}', font=('Arial', 12, 'bold'))
    label_total_estoque.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID ESTOQUE', 'DATA DE ENTRADA', 'ID FORNECEDOR', 'LOTE', 'NOME', 'TIPO', 'COR', 'TAMANHO', 'QUANTIDADE', 'DATA DE VALIDADE')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [20, 40, 20, 20, 150, 30, 40, 20, 60, 60]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_pedido(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Pedidos')
    janelaexibicao.geometry('1200x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')

            if filtro is None or str(filtro) == valores[0]:
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PEDIDO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM PEDIDO')
    total_pedido = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_pedido = tk.Label(janelaexibicao, text=f'Total de Pedido: {total_pedido}', font=('Arial', 12, 'bold'))
    label_total_pedido.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID PEDIDO', 'DATA DE EMISSÃO', 'ID CLIENTE', 'ID PRODUTO', 'ID VENDEDOR', 'QUANTIDADE', 'DATA DE ENTREGA', 'MODO DE PAGAMENTO', 'PARCELAS', 'PREÇO TOTAL')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [20, 70, 20, 20, 20, 30, 60, 60, 20, 60]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    janelaexibicao.mainloop()


def criar_janela_exibicao_material_consumo(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Material')
    janelaexibicao.geometry('1200x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')

            if filtro is None or str(filtro) == valores[2]: # PARA  PESQUISA EXATA. SE EU QUISER PARCIALMENTE in valores:
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        total_material_filtro = len(dados_filtrados)
        label_total_material['text'] = f'Total de Material: {total_material}                     (Com Filtro: {total_material_filtro})'

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PEDIDO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM MATERIAL_PRA_CONSUMO')
    total_material = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_material = tk.Label(janelaexibicao, text=f'Total de Material: {total_material}', font=('Arial', 12, 'bold'))
    label_total_material.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID MATERIAL', 'DATA DE SAÍDA', 'ID PEDIDO', 'ID ESTOQUE', 'QUANTIDADE')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [20, 80, 20, 20, 40]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_fabricacao(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Fabricação')
    janelaexibicao.geometry('1200x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        if filtro is not None:
            for resultado in dados:
                valores = resultado.split(', ')

                if str(filtro).upper() == valores[5].upper() or str(filtro).upper() == valores[14].upper():
                    # Para correspondência exata quando o filtro é definido para valores[5] ou valores[14]
                    valores_upper = [valor.upper() for valor in valores]
                    treeview.insert('', 'end', values=valores_upper)
                    dados_filtrados.append(resultado)

        total_fabri_filtro = len(dados_filtrados)
        label_total_fabricacao['text'] = f'Total de Produto em Fabricação: {total_fabricacao}                     (Com Filtro: {total_fabri_filtro})'

        if not dados_filtrados and filtro is not None:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PEDIDO/STATUS)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM PRODUTO_EM_FABRICAÇAO')
    total_fabricacao = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_fabricacao = tk.Label(janelaexibicao, text=f'Total de Produto em Fabricação: {total_fabricacao}', font=('Arial', 12, 'bold'))
    label_total_fabricacao.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID FABRICAÇÃO', 'DATA', 'NOME', 'CPF', 'CNPJ', 'ID PEDIDO', 'ID VENDEDOR', 'QUANTIDADE', 'DATA ENTREGA', 'PRODUTO', 'TIPO', 'COR', 'TURBO', 'PORTAS', 'STATUS')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [10, 40, 110, 50, 70, 10, 10, 10, 50, 60, 60, 10, 5, 10, 40]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_historico(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Histórico')
    janelaexibicao.geometry('1200x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        if filtro is not None:
            for resultado in dados:
                valores = resultado.split(', ')

                if str(filtro).upper() == valores[1].upper():
                    valores_upper = [valor.upper() for valor in valores]
                    treeview.insert('', 'end', values=valores_upper)
                    dados_filtrados.append(resultado)

        total_historico_filtro = len(dados_filtrados)
        label_total_historico['text'] = f'Total de Histórico Meta: {total_historico}                     (Com Filtro: {total_historico_filtro})'

        if not dados_filtrados and filtro is not None:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID VENDEDOR)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM HISTORICO_META')
    total_historico = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_historico = tk.Label(janelaexibicao, text=f'Total de Histórico Meta: {total_historico}', font=('Arial', 12, 'bold'))
    label_total_historico.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID HISTORICO', 'ID VENDEDOR', 'VALOR META', 'ANDAMENTO', 'DATA FINAL META')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [60, 60, 60, 60, 60]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def criar_janela_exibicao_folha(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Folha de Pagamento')
    janelaexibicao.geometry('800x400')

    def exibir_dados_no_treeview(filtro_id_vendedor=None, filtro_data=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        for resultado in dados:
            valores = resultado.split(', ')
            id_vendedor, data = valores[3], valores[1]

            # Converte a data para o formato do banco
            data_banco = datetime.strptime(data, '%Y-%m-%d')

            # Converte a data do filtro para o mesmo formato
            filtro_data_banco = None
            if filtro_data:
                try:
                    # Ajusta o formato da data do calendário
                    filtro_data_banco = datetime.strptime(filtro_data, '%m/%d/%y')
                except ValueError:
                    # Tratamento de erro caso a conversão falhe
                    label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
                    return

            # Verifica se o resultado atende ao filtro de ID do vendedor
            filtro_id_vendedor = filtro_id_vendedor.lower() if filtro_id_vendedor else None
            id_vendedor_lower = id_vendedor.lower()
            if (filtro_id_vendedor is None or filtro_id_vendedor in id_vendedor_lower) and \
                    (filtro_data_banco is None or filtro_data_banco.date() == data_banco.date()):
                treeview.insert('', 'end', values=valores)
                dados_filtrados.append(resultado)

        # Após o loop, atualize o rótulo do total para refletir o total após o filtro
        total_meta_filtro = len(dados_filtrados)
        label_total_meta['text'] = f'Total de Folhas: {total_meta}                     (Com Filtro: {total_meta_filtro})'

        if not dados_filtrados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Rótulo para indicar o campo de filtro por ID do vendedor
    label_id_vendedor = tk.Label(janelaexibicao, text='Filtrar Por ID Funciónario:')
    label_id_vendedor.pack(side='top', pady=5)

    # Caixa de entrada para o filtro por ID do vendedor
    entry_filtro_id_vendedor = tk.Entry(janelaexibicao, width=10)
    entry_filtro_id_vendedor.pack(side='top', pady=5)

    # Caixa de entrada para o filtro por data (calendário)
    calendar_label = tk.Label(janelaexibicao, text='Filtrar Por Data:')
    calendar_label.pack(side='top', pady=5)

    # Widget de calendário
    entry_filtro_data = DateEntry(janelaexibicao, width=12, background='darkblue', foreground='white', borderwidth=2,
                                  date_pattern='mm/dd/yy')
    entry_filtro_data.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'),
                               command=lambda: exibir_dados_no_treeview(entry_filtro_id_vendedor.get(),
                                                                        entry_filtro_data.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM folha_de_pagamento')
    total_meta = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de clientes
    label_total_meta = tk.Label(janelaexibicao, text=f'Total de Folhas: {total_meta}', font=('Arial', 12, 'bold'))
    label_total_meta.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID FOLHA', 'DATA DO PAGAMENTO', 'MÊS DE REFERÊNCIA', 'ID FUNCIONÁRIO', 'SALÁRIO BRUTO', 'INSS', 'SALÁRIO LIQUIDO')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [10, 60, 20, 10, 50, 10, 50]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    janelaexibicao.mainloop()


# ########################################### SELEÇÃO DOS DADOS NO BANCO DE DADOS ######################################

def selecionar_dados_funcionario():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM FUNCIONARIO''')
        resultados = []

        for linha in cursor.fetchall():
            idfuncionario, data_de_admissao, nome, cpf, pis, rua, numero, bairro, cidade, estado, ddd, telefone, sexo, idcargo, status, salario_bruto, salario_bruto_com_comissao = linha
            resultado_str = f'{idfuncionario}, {data_de_admissao}, {nome}, {cpf}, {pis}, {rua}, {numero}, {bairro}, {cidade}, {estado}, {ddd}, {telefone}, {sexo}, {idcargo}, {status}, {round(salario_bruto, 2)}, {round(salario_bruto_com_comissao, 2)}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_funcionario(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_cliente():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT IDCLIENTE, DATA, NOME, DATA_DE_NASCIMENTO, CPF, CNPJ, EMAIL, RUA, NUMERO, BAIRRO, CIDADE, ESTADO, DDD, TELEFONE FROM CLIENTE''')
        resultados = []

        for linha in cursor.fetchall():
            idcliente, data, nome, data_de_nascimento, cpf, cnpj, email, rua, numero, bairro, cidade, estado, ddd, telefone = linha
            resultado_str = f'{idcliente}, {nome}, {data_de_nascimento}, {cpf}, {cnpj}, {email}, {rua}, {numero}, {bairro}, {cidade}, {estado}, {ddd}, {telefone}'
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_cliente(resultados)# ao clicar no botao, ele vai executar o select a estou associando o resultado a def pra criar a janela

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_cargo():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM CARGO''')
        resultados = []

        for linha in cursor.fetchall():
            idcargo, nome = linha
            resultado_str = f'{idcargo}, {nome}'
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_cargo(resultados)

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_estado():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM ESTADO''')
        resultados = []

        for linha in cursor.fetchall():
            idestado, uf = linha
            resultado_str = f'{idestado}, {uf}'
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_estado(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_area():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM AREA_DE_ATUAÇAO_VENDEDOR''')
        resultados = []

        for linha in cursor.fetchall():
            idvendedor, idfuncionario, idestado = linha
            resultado_str = f'{idvendedor}, {idfuncionario}, {idestado}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_area(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_meta():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM META_VENDEDOR''')
        resultados = []

        for linha in cursor.fetchall():
            idmeta, idvendedor, valor_meta, andamento, datafinal = linha
            resultado_str = f'{idmeta}, {idvendedor}, {round(valor_meta, 2)}, {round(andamento, 2)}, {datafinal}'
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_meta(resultados)

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_produto():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM PRODUTO''')
        resultados = []

        for linha in cursor.fetchall():
            idproduto, nome, tipo, cor, turbo, portas, preco = linha
            resultado_str = f'{idproduto}, {nome}, {tipo}, {cor}, {turbo}, {portas}, {round(preco, 2)}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_produto(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_fornecedor():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM FORNECEDOR''')
        resultados = []

        for linha in cursor.fetchall():
            idfornecedor, data, nome, tipomaterial, cnpj, ddd, telefone, email, numero, rua, bairro, cidade, estado, situacao = linha
            resultado_str = f'{idfornecedor}, {data}, {nome}, {tipomaterial}, {cnpj}, {ddd}, {telefone}, {email}, {numero}, {rua}, {bairro}, {cidade}, {estado}, {situacao}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_fornecedor(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_estoque():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM ESTOQUE_DE_MATERIAL''')
        resultados = []

        for linha in cursor.fetchall():
            idestoque, data, id_fornecedor, lote, nome, tipo, cor, tamanho, quantidade, data_de_validade = linha
            resultado_str = f'{idestoque}, {data}, {id_fornecedor}, {lote}, {nome}, {tipo}, {cor}, {tamanho}, {quantidade}, {data_de_validade}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_estoque(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_pedido():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM PEDIDO''')
        resultados = []

        for linha in cursor.fetchall():
            idpedido, datadeemissao, idcliente, idproduto, idvendedor, quantidade, dataentrega, modopagamento, parcelas, precototal = linha
            resultado_str = f'{idpedido}, {datadeemissao}, {idcliente}, {idproduto}, {idvendedor}, {quantidade}, {dataentrega}, {modopagamento}, {parcelas}, {precototal}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_pedido(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_material_consumo():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM MATERIAL_PRA_CONSUMO''')
        resultados = []

        for linha in cursor.fetchall():
            idmaterial, datasaida, idpedido, idestoque, quantidade = linha
            resultado_str = f'{idmaterial}, {datasaida}, {idpedido}, {idestoque}, {quantidade}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_material_consumo(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_fabricacao():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM PRODUTO_EM_FABRICAÇAO''')
        resultados = []

        for linha in cursor.fetchall():
            id_frabricacao, data, nome, cpf, cnpj, id_pedido, id_vendedor, quantidade, data_de_entrega, nomep, tipo, cor, turbo, portas, status= linha
            resultado_str = f'{id_frabricacao}, {data}, {nome}, {cpf}, {cnpj}, {id_pedido}, {id_vendedor}, {quantidade}, {data_de_entrega}, {nomep}, {tipo}, {cor}, {turbo}, {portas}, {status}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_fabricacao(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_historico():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM HISTORICO_META''')
        resultados = []

        for linha in cursor.fetchall():
            idhistorico, idvendedor, valormeta, andamento, datafinalmeta = linha
            resultado_str = f'{idhistorico}, {idvendedor}, {valormeta}, {andamento}, {datafinalmeta} ' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_historico(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def selecionar_dados_folha():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM FOLHA_DE_PAGAMENTO''')
        resultados = []

        for linha in cursor.fetchall():
            idfolha, data, mes, idfuncionario, salariobruto, inss, salarioliquido = linha
            resultado_str = f'{idfolha}, {data}, {mes}, {idfuncionario}, {salariobruto}, {inss}, {salarioliquido}' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_folha(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


# ############################################ INSERÇÃO DOS DADOS NO BANCO DE DADOS ####################################

def cadastrar_cargo(nome):
    try:

        conn = conectar_banco()
        cursor = conn.cursor()

        if not nome:
            messagebox.showwarning('Campo Vazio', 'Por favor, Preencha o campo vazio!')
            return
        else:
            cursor.execute('''INSERT INTO CARGO (NOME) VALUES (?)''', nome)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos!')
            limpar_tela_cargo()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inserir dados! {erro}')


def inserir_dados_estado(uf):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not uf:
            messagebox.showwarning('Campo Vazio', 'Por favor, Preencha o campo vazio!')
            return
        else:
            cursor.execute('''INSERT INTO ESTADO (UF) VALUES (?)''', uf)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos')
            limpar_tela_estado()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inserir os dados! {erro}')


def cadastrar_area_atuacao(id_funcionario, id_estado):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not id_funcionario or not id_estado:
            messagebox.showwarning('Campos Vazios', 'Por favor, Preencha os campos vazios')
            return

        else:
            cursor.execute('''INSERT INTO AREA_DE_ATUAÇAO_VENDEDOR (ID_FUNCIONARIO, ID_ESTADO) VALUES (?, ?)''', id_funcionario, id_estado)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos')
            limpar_tela_area()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Erro ao inserir os dados {erro}')


def cadastrar_meta(idvendedor, valor_meta, data_final):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idvendedor or not valor_meta or not data_final:
            messagebox.showwarning('Campos Vazios', 'Por Favor, Preencha todos os campos')
            return

        if not re.match(r'\d{2}\/\d{2}\/\d{4}', data_final):
            messagebox.showwarning('Formato Data', 'Por favor, Preencha a DATA como nesse exemplo: (00/00/0000)')
            return
        else:
            cursor.execute('''INSERT INTO META_VENDEDOR (ID_VENDEDOR, VALOR_META, ANDAMENTO, DATA_FINAL_META) VALUES (?, ?, 0, ?)''', idvendedor, valor_meta, data_final)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos com sucesso!')
            limpar_tela_meta()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inseridos os dados! {erro}')


def cadastrar_produto(nome, tipo, cor, turbo, portas, preco):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not nome or not tipo or not cor or not turbo or not portas or not preco:
            messagebox.showwarning('Campos Vazios', 'Por Favor, Preencha todos os campos')
            return

        else:
            cursor.execute('''INSERT INTO PRODUTOS (NOME, TIPO, COR, TURBO, PORTAS, PREÇO) VALUES (?, ?, ?, ?, ?, ?)''', nome, tipo, cor, turbo, portas, preco )
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos com sucesso!')
            limpar_tela_produto()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inseridos os dados! {erro}')


def inserir_dados_fornecedor(nome, tipomaterial, cnpj, ddd, telefone, email, numero, rua, bairro, cidade, estado, situacao):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not nome or not tipomaterial or not cnpj or not ddd or not telefone or not email or not rua or not bairro or not cidade or not estado or not situacao:
            messagebox.showwarning('Campos Vazios', 'Por favor, Preencha todos os campos')
            return

        if not re.match(r'\d{2}\.\d{3}\.\d{3}\/\d{4}\-\d{2}', cnpj):
            messagebox.showwarning('Formato CNPJ', 'O Formato do CNPJ deve ser: (12.345.678/0003-00) e conter 14 numeros!')
            return

        if not re.match(r'[^@]+@[^@]+\.[^@]+', email):
            messagebox.showwarning('E-MAIL', 'Por favor, Verifique se no campo E-MAIL, contém: ("@", ".")')
            return

        else:
            cursor.execute('''INSERT INTO FORNECEDOR (DATA_DE_ENTRADA, NOME, TIPO_DE_MATERIAL, CNPJ, DDD, TELEFONE, EMAIL, NUMERO, RUA, BAIRRO, CIDADE, ESTADO, SITUAÇAO VALUES(GETDATE(), ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?))''', nome, tipomaterial, cnpj, ddd, telefone, email, numero, rua, bairro, cidade, estado, situacao)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os dados foram inseridos com sucesso!')
            limpar_tela_fornecedor()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inserir os dados {erro}')


def inserir_cadastro_estoque(idfornecedor, lote, nome, tipo, cor, tamanho, quantidade, datavalidade):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idfornecedor or not lote or not nome or not tipo or not cor or not tamanho or not quantidade:
            messagebox.showwarning('Campos Vazios', 'Por favor, preencha todos os campos!')
            return

        if datavalidade != '' and not re.match(r'\d{2}\/\d{2}\/\d{4}', datavalidade):
            messagebox.showwarning('Formato Data', 'Por favor, o campo Data deve seguir o exemplo: (00/00/000)')
            return
        if datavalidade == '':
            datavalidade = None

        cursor.execute('''
            INSERT INTO ESTOQUE_DE_MATERIAL (DATA_DE_ENTRADA, ID_FORNECEDOR, LOTE, NOME, TIPO, COR, TAMANHO, QUANTIDADE, DATA_DE_VALIDADE)
            VALUES (GETDATE(), ?, ?, ?, ?, ?, ?, ?, ?)
        ''', idfornecedor, lote, nome, tipo, cor, tamanho, quantidade, datavalidade)
        conn.commit()
        messagebox.showinfo('Sucesso', 'Os dados foram inseridos!')
        limpar_tela_cadastro_estoque()
    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Os dados não foram inseridos {erro}')


def inserir_dados_pedido(idcliente, idproduto, idvendedor, quantidade, data_entrega, modopagamento, parcelas):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idcliente or not idproduto or not idvendedor or not quantidade or not data_entrega or not modopagamento or not parcelas:
            messagebox.showwarning('Campos Vazios', 'Por favor, preencha todos os campos!')
            return

        if not re.match(r'\d{2}\/\d{2}\/\d{4}', data_entrega):
            messagebox.showwarning('Data de Entrega', 'O formato do campo Data Entrega deve seguir o exemplo: (00/00/0000)')
            return

        cursor.execute('''
            INSERT INTO PEDIDO (DATA_DE_EMISSAO, ID_CLIENTE, ID_PRODUTO, ID_VENDEDOR, QUANTIDADE, DATA_DE_ENTREGA, MODO_DE_PAGAMENTO, PARCELAS, PREÇO_TOTAL)
            VALUES (GETDATE(), ?, ?, ?, ?, ?, ?, ?, 0)
        ''', idcliente, idproduto, idvendedor, quantidade, data_entrega, modopagamento, parcelas)
        conn.commit()
        messagebox.showinfo('Sucesso', 'Os dados foram inseridos!')
        limpar_tela_cadastro_pedido()
    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Os dados não foram inseridos! {erro}')


def inserir_dados_retirada(idpedido, idestoque, quantidade):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idpedido or not idestoque or not quantidade:
            messagebox.showwarning('Campos Vazios', 'Por favor, preencha todos os campos!')
            return

        if int(quantidade) < 1:
            messagebox.showwarning('Quantidade', 'A quantidade deve conter pelo menos um produto!')
            return

        # verificando se a o material existe
        cursor.execute('''SELECT IDESTOQUE FROM ESTOQUE_DE_MATERIAL WHERE IDESTOQUE = ?''', idestoque)
        resultado2 = cursor.fetchone()
        if not resultado2:
            messagebox.showwarning('Material', 'Material não encontrado, verifique se ele existe!')
            return
        # Verificando se o pedido existe
        cursor.execute('''SELECT IDPEDIDO FROM PEDIDO WHERE IDPEDIDO = ?''', idpedido)
        resultado = cursor.fetchone()
        if not resultado:
            messagebox.showwarning('PEDIDO', 'Pedido não encontrado, verifique e tente novamente!')
            return
        else:
            cursor.execute('''INSERT INTO MATERIAL_PRA_CONSUMO (DATA_DE_SAIDA, ID_PEDIDO, ID_ESTOQUE, QUANTIDADE) VALUES (GETDATE(), ?, ?, ?)''', idpedido, idestoque, quantidade)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Os produtos foram retirados com sucesso!')
            limpar_tela_saida_material()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível retirar o produto! {erro}')


def inserir_dados_folha(idfuncionario, calculo):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idfuncionario or not calculo:
            messagebox.showwarning('Campos Vazios', 'Por favor, preencha todos os campos!')
            return
        else:
            # Passando o idpedido como parâmetro para o procedimento armazenado
            cursor.execute('EXEC SP_PAGAMENTO_FUNCIONARIO ?, ?', (idfuncionario, calculo))
            conn.commit()
            messagebox.showinfo('Sucesso', 'Folha de pagamento gerada!')
            limpar_tela_pagamento_folha()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível gerar Folha de pagamento {erro}')


# ###################################### ATUALIZAÇAO DE DADOS ###########################################################

entry_nomeup = entry_nascimentoup = entry_cnpjup = entry_emailup = entry_ruaup = entry_numeroup = None
entry_bairroup = entry_cidadeup = entry_estadoup = entry_dddup = entry_telefoneup = entry_cpfup = None


def update_cliente():

    global entry_nomeup, entry_nascimentoup, entry_cnpjup, entry_emailup, entry_ruaup, entry_numeroup
    global entry_bairroup, entry_cidadeup, entry_estadoup, entry_dddup, entry_telefoneup, entry_cpfup

    janelaupdatecliente = tk.Tk()
    janelaupdatecliente.title('Atualizar Cliente')
    janelaupdatecliente.geometry('400x600')

    label_nomeup = tk.Label(janelaupdatecliente, text='NOME:')
    label_nomeup.grid(row=0, column=0, padx=10, pady=10)
    entry_nomeup = ttk.Entry(janelaupdatecliente)
    entry_nomeup.grid(row=0, column=1, padx=10, pady=10)

    label_nascimentoup = tk.Label(janelaupdatecliente, text='DATA DE NASCIMENTO:')
    label_nascimentoup.grid(row=1, column=0, padx=10, pady=10)
    entry_nascimentoup = ttk.Entry(janelaupdatecliente)
    entry_nascimentoup.grid(row=1, column=1, padx=10, pady=10)

    label_cnpjup = tk.Label(janelaupdatecliente, text='CNPJ:')
    label_cnpjup.grid(row=2, column=0, padx=10, pady=10)
    entry_cnpjup = ttk.Entry(janelaupdatecliente)
    entry_cnpjup.grid(row=2, column=1, padx=10, pady=10)

    label_emailup = tk.Label(janelaupdatecliente, text='E-MAIL:')
    label_emailup.grid(row=3, column=0, padx=10, pady=10)
    entry_emailup = ttk.Entry(janelaupdatecliente)
    entry_emailup.grid(row=3, column=1, padx=10, pady=10)

    label_ruaup = tk.Label(janelaupdatecliente, text='RUA:')
    label_ruaup.grid(row=4, column=0, padx=10, pady=10)
    entry_ruaup = ttk.Entry(janelaupdatecliente)
    entry_ruaup.grid(row=4, column=1, padx=10, pady=10)

    label_numeroup = tk.Label(janelaupdatecliente, text='NUMERO:')
    label_numeroup.grid(row=5, column=0, padx=10, pady=10)
    entry_numeroup = ttk.Entry(janelaupdatecliente)
    entry_numeroup.grid(row=5, column=1, padx=10, pady=10)

    label_bairroup = tk.Label(janelaupdatecliente, text='BAIRRO:')
    label_bairroup.grid(row=6, column=0, padx=10, pady=10)
    entry_bairroup = ttk.Entry(janelaupdatecliente)
    entry_bairroup.grid(row=6, column=1, padx=10, pady=10)

    label_cidadeup = tk.Label(janelaupdatecliente, text='CIDADE:')
    label_cidadeup.grid(row=7, column=0, padx=10, pady=10)
    entry_cidadeup = ttk.Entry(janelaupdatecliente)
    entry_cidadeup.grid(row=7, column=1, padx=10, pady=10)

    label_estadoup = tk.Label(janelaupdatecliente, text='ESTADO:')
    label_estadoup.grid(row=8, column=0, padx=10, pady=10)
    entry_estadoup = ttk.Entry(janelaupdatecliente)
    entry_estadoup.grid(row=8, column=1, padx=10, pady=10)

    label_dddup = tk.Label(janelaupdatecliente, text='DDD:')
    label_dddup.grid(row=9, column=0, padx=10, pady=10)
    entry_dddup = ttk.Entry(janelaupdatecliente)
    entry_dddup.grid(row=9, column=1, padx=10, pady=10)

    label_telefoneup = tk.Label(janelaupdatecliente, text='TELEFONE:')
    label_telefoneup.grid(row=10, column=0, padx=10, pady=10)
    entry_telefoneup = ttk.Entry(janelaupdatecliente)
    entry_telefoneup.grid(row=10, column=1, padx=10, pady=10)

    label_cpfup = tk.Label(janelaupdatecliente, text='PARAMETRO CPF:', bg='green', fg='white')
    label_cpfup.grid(row=11, column=0, padx=10, pady=10)
    entry_cpfup = ttk.Entry(janelaupdatecliente)
    entry_cpfup.grid(row=11, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdatecliente, text='*')
    desc_cpfup.grid(row=11, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdatecliente, text='Atualizar Cadastro', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_clientes(entry_nomeup.get(), entry_nascimentoup.get(), entry_cnpjup.get(), entry_emailup.get(), entry_ruaup.get(), entry_numeroup.get(), entry_bairroup.get(), entry_cidadeup.get(), entry_estadoup.get(), entry_dddup.get(), entry_telefoneup.get(), entry_cpfup.get()))
    botao_saida_material.grid(row=12, column=1, padx=10, pady=10)

    janelaupdatecliente.mainloop()


def update_clientes(nome, data_de_nascimento, cnpj, email, rua, numero, bairro, cidade, estado, ddd, telefone, cpf):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if cpf:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM CLIENTE WHERE CPF = ?''', cpf)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('CPF não encontrado, verifique e tente novamente!')
            if nome:
                cursor.execute('''UPDATE CLIENTE SET NOME = ? WHERE CPF = ? ''', (nome.upper(), cpf)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if data_de_nascimento:
                cursor.execute('''UPDATE CLIENTE SET DATA_DE_NASCIMENTO = ? WHERE CPF = ? ''', (data_de_nascimento, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if cnpj:
                cursor.execute('''UPDATE CLIENTE SET CNPJ = ? WHERE CPF = ? ''', (cnpj, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if email:
                cursor.execute('''UPDATE CLIENTE SET EMAIL = ? WHERE CPF = ? ''', (email, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if rua:
                cursor.execute('''UPDATE CLIENTE SET RUA = ? WHERE CPF = ? ''', (rua.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if numero:
                cursor.execute('''UPDATE CLIENTE SET NUMERO = ? WHERE CPF = ? ''', (numero, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if bairro:
                cursor.execute('''UPDATE CLIENTE SET BAIRRO = ? WHERE CPF = ? ''', (bairro.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if cidade:
                cursor.execute('''UPDATE CLIENTE SET CIDADE = ? WHERE CPF = ? ''', (cidade.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if estado:
                cursor.execute('''UPDATE CLIENTE SET ESTADO = ? WHERE CPF = ? ''', (estado.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if ddd:
                cursor.execute('''UPDATE CLIENTE SET DDD = ? WHERE CPF = ? ''', (ddd, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
            if telefone:
                cursor.execute('''UPDATE CLIENTE SET TELEFONE = ? WHERE CPF = ? ''', (telefone, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_cliente()
        else:
            messagebox.showinfo('CPF', 'O PARAMETRO CPF não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_nomeup = entry_pisup = entry_ruaup = entry_nup = entry_bairroup = entry_cidadeup = entry_estadoup = None
entry_dddup = entry_telefoneup = entry_idcargo = entry_status = entry_idsalario = entry_cpfup1 = None


def update_funcionario():

    global entry_nomeup, entry_pisup, entry_ruaup, entry_nup, entry_bairroup, entry_cidadeup, entry_estadoup
    global entry_dddup, entry_telefoneup, entry_idcargo, entry_status, entry_idsalario, entry_cpfup1

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Funcionario')
    janelaupdate.geometry('400x600')

    label_nomeup = tk.Label(janelaupdate, text='NOME:')
    label_nomeup.grid(row=0, column=0, padx=10, pady=10)
    entry_nomeup = ttk.Entry(janelaupdate)
    entry_nomeup.grid(row=0, column=1, padx=10, pady=10)

    label_pisup = tk.Label(janelaupdate, text='PIS:')
    label_pisup.grid(row=1, column=0, padx=10, pady=10)
    entry_pisup = ttk.Entry(janelaupdate)
    entry_pisup.grid(row=1, column=1, padx=10, pady=10)

    label_ruaup = tk.Label(janelaupdate, text='RUA:')
    label_ruaup.grid(row=2, column=0, padx=10, pady=10)
    entry_ruaup = ttk.Entry(janelaupdate)
    entry_ruaup.grid(row=2, column=1, padx=10, pady=10)

    label_nup = tk.Label(janelaupdate, text='Nº:')
    label_nup.grid(row=3, column=0, padx=10, pady=10)
    entry_nup = ttk.Entry(janelaupdate)
    entry_nup.grid(row=3, column=1, padx=10, pady=10)

    label_bairroup = tk.Label(janelaupdate, text='BAIRRO:')
    label_bairroup.grid(row=4, column=0, padx=10, pady=10)
    entry_bairroup = ttk.Entry(janelaupdate)
    entry_bairroup.grid(row=4, column=1, padx=10, pady=10)

    label_cidadeup = tk.Label(janelaupdate, text='CIDADE:')
    label_cidadeup.grid(row=5, column=0, padx=10, pady=10)
    entry_cidadeup = ttk.Entry(janelaupdate)
    entry_cidadeup.grid(row=5, column=1, padx=10, pady=10)

    label_estadoup = tk.Label(janelaupdate, text='ESTADO:')
    label_estadoup.grid(row=6, column=0, padx=10, pady=10)
    entry_estadoup = ttk.Entry(janelaupdate)
    entry_estadoup.grid(row=6, column=1, padx=10, pady=10)

    label_dddup = tk.Label(janelaupdate, text='DDD:')
    label_dddup.grid(row=7, column=0, padx=10, pady=10)
    entry_dddup = ttk.Entry(janelaupdate)
    entry_dddup.grid(row=7, column=1, padx=10, pady=10)

    label_telefoneup = tk.Label(janelaupdate, text='TELEFONE:')
    label_telefoneup.grid(row=8, column=0, padx=10, pady=10)
    entry_telefoneup = ttk.Entry(janelaupdate)
    entry_telefoneup.grid(row=8, column=1, padx=10, pady=10)

    label_idcargo = tk.Label(janelaupdate, text='ID CARGO:')
    label_idcargo.grid(row=9, column=0, padx=10, pady=10)
    entry_idcargo = ttk.Entry(janelaupdate)
    entry_idcargo.grid(row=9, column=1, padx=10, pady=10)

    label_status = tk.Label(janelaupdate, text='STATUS:')
    label_status.grid(row=10, column=0, padx=10, pady=10)
    entry_status = ttk.Entry(janelaupdate)
    entry_status.grid(row=10, column=1, padx=10, pady=10)

    label_idsalario = tk.Label(janelaupdate, text='SALÁRIO:')
    label_idsalario.grid(row=11, column=0, padx=10, pady=10)
    entry_idsalario = ttk.Entry(janelaupdate)
    entry_idsalario.grid(row=11, column=1, padx=10, pady=10)

    label_cpfup1 = tk.Label(janelaupdate, text='PARAMETRO CPF:', bg='green', fg='white')
    label_cpfup1.grid(row=12, column=0, padx=10, pady=10)
    entry_cpfup1 = ttk.Entry(janelaupdate)
    entry_cpfup1.grid(row=12, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=12, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Cadastro', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_funcionarios(entry_nomeup.get(), entry_pisup.get(), entry_ruaup.get(), entry_nup.get(), entry_bairroup.get(), entry_cidadeup.get(), entry_estadoup.get(), entry_dddup.get(), entry_telefoneup.get(), entry_idcargo.get(), entry_status.get(), entry_idsalario.get(), entry_cpfup1.get()))
    botao_saida_material.grid(row=13, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_funcionarios(nome, pis, rua, n, bairro, cidade, estado, ddd, telefone, idcargo, status, salario, cpf):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if cpf:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM FUNCIONARIO WHERE CPF = ?''', cpf)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('CPF', 'CPF não encontrado, verifique e tente novamente!')
            if nome:
                cursor.execute('''UPDATE FUNCIONARIO SET NOME = ? WHERE CPF = ? ''', (nome.upper(), cpf)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if pis:
                cursor.execute('''UPDATE FUNCIONARIO SET PIS = ? WHERE CPF = ? ''', (pis, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if rua:
                cursor.execute('''UPDATE FUNCIONARIO SET RUA = ? WHERE CPF = ? ''', (rua.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if n:
                cursor.execute('''UPDATE FUNCIONARIO SET NUMERO = ? WHERE CPF = ? ''', (n, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if bairro:
                cursor.execute('''UPDATE FUNCIONARIO SET BAIRRO = ? WHERE CPF = ? ''', (bairro.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if cidade:
                cursor.execute('''UPDATE FUNCIONARIO SET CIDADE = ? WHERE CPF = ? ''', (cidade.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if estado:
                cursor.execute('''UPDATE FUNCIONARIO SET ESTADO = ? WHERE CPF = ? ''', (estado.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if ddd:
                cursor.execute('''UPDATE FUNCIONARIO SET DDD = ? WHERE CPF = ? ''', (ddd, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if telefone:
                cursor.execute('''UPDATE FUNCIONARIO SET TELEFONE = ? WHERE CPF = ? ''', (telefone, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if idcargo:
                cursor.execute('''UPDATE FUNCIONARIO SET ID_CARGO = ? WHERE CPF = ? ''', (idcargo, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if status:
                cursor.execute('''UPDATE FUNCIONARIO SET STATUS = ? WHERE CPF = ? ''', (status.upper(), cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
            if salario:
                cursor.execute('''UPDATE FUNCIONARIO SET SALARIO_BRUTO = ? WHERE CPF = ? ''', (salario, cpf))
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_funcionario()
        else:
            messagebox.showinfo('CPF', 'O PARAMETRO CPF não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_id_estado = entry_idvendedor = None


def update_area_vendedor():

    global entry_id_estado, entry_idvendedor

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Área Vendedor')
    janelaupdate.geometry('380x180')

    label_id_estado = tk.Label(janelaupdate, text='ID ESTADO:')
    label_id_estado.grid(row=0, column=0, padx=10, pady=10)
    entry_id_estado = ttk.Entry(janelaupdate)
    entry_id_estado.grid(row=0, column=1, padx=10, pady=10)

    label_idvendedor = tk.Label(janelaupdate, text='PARAMETRO ID VENDEDOR:', bg='green', fg='white')
    label_idvendedor.grid(row=1, column=0, padx=10, pady=10)
    entry_idvendedor = ttk.Entry(janelaupdate)
    entry_idvendedor.grid(row=1, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=1, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Área', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_area_vendedores(entry_id_estado.get(), entry_idvendedor.get()))
    botao_saida_material.grid(row=13, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_area_vendedores(id_estado, idvendedor):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idvendedor:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM AREA_DE_ATUAÇAO_VENDEDOR WHERE IDVENDEDOR = ?''', idvendedor)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID VENDEDOR', 'O ID VENDEDOR não encontrado, verifique e tente novamente!')
            if id_estado:
                cursor.execute('''UPDATE AREA_DE_ATUAÇAO_VENDEDOR SET ID_ESTADO = ? WHERE IDVENDEDOR = ? ''', (id_estado, idvendedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_area()
        else:
            messagebox.showinfo('ID VENDEDOR', 'O PARAMETRO ID VENDEDOR não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_nomepro = entry_desc_pro = entry_cor_pro = entry_turbo_pro = entry_portas_pro = entry_preco_pro = entry_idproduto = None


def update_produto():

    global entry_nomepro, entry_desc_pro, entry_cor_pro, entry_turbo_pro, entry_portas_pro, entry_preco_pro, entry_idproduto

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Produto')
    janelaupdate.geometry('400x400')

    label_nomepro = tk.Label(janelaupdate, text='NOME:')
    label_nomepro.grid(row=0, column=0, padx=10, pady=10)
    entry_nomepro = ttk.Entry(janelaupdate)
    entry_nomepro.grid(row=0, column=1, padx=10, pady=10)

    label_desc_pro = tk.Label(janelaupdate, text='DESCRIÇÃO:')
    label_desc_pro.grid(row=1, column=0, padx=10, pady=10)
    entry_desc_pro = ttk.Entry(janelaupdate)
    entry_desc_pro.grid(row=1, column=1, padx=10, pady=10)

    label_cor_pro = tk.Label(janelaupdate, text='COR:')
    label_cor_pro.grid(row=2, column=0, padx=10, pady=10)
    entry_cor_pro = ttk.Entry(janelaupdate)
    entry_cor_pro.grid(row=2, column=1, padx=10, pady=10)

    label_turbo_pro = tk.Label(janelaupdate, text='TURBO:')
    label_turbo_pro.grid(row=3, column=0, padx=10, pady=10)
    entry_turbo_pro = ttk.Entry(janelaupdate)
    entry_turbo_pro.grid(row=3, column=1, padx=10, pady=10)

    label_portas_pro = tk.Label(janelaupdate, text='PORTAS:')
    label_portas_pro.grid(row=4, column=0, padx=10, pady=10)
    entry_portas_pro = ttk.Entry(janelaupdate)
    entry_portas_pro.grid(row=4, column=1, padx=10, pady=10)

    label_preco_pro = tk.Label(janelaupdate, text='PREÇO:')
    label_preco_pro.grid(row=5, column=0, padx=10, pady=10)
    entry_preco_pro = ttk.Entry(janelaupdate)
    entry_preco_pro.grid(row=5, column=1, padx=10, pady=10)

    label_idproduto = tk.Label(janelaupdate, text='PARAMETRO ID PRODUTO:', bg='green', fg='white')
    label_idproduto.grid(row=6, column=0, padx=10, pady=10)
    entry_idproduto = ttk.Entry(janelaupdate)
    entry_idproduto.grid(row=6, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=6, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Produto', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_produtos(entry_nomepro.get(), entry_desc_pro.get(), entry_cor_pro.get(), entry_turbo_pro.get(), entry_portas_pro.get(), entry_preco_pro.get(), entry_idproduto.get()))
    botao_saida_material.grid(row=7, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_produtos(nome, descricao, cor, turbo, portas, preco, idproduto):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idproduto:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM PRODUTO WHERE IDPRODUTO = ?''', idproduto)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID PRODUTO', ' ID PRODUTO não encontrado, verifique e tente novamente!')
            if nome:
                cursor.execute('''UPDATE PRODUTO SET NOME = ? WHERE IDPRODUTO = ? ''', (nome, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
            if descricao:
                cursor.execute('''UPDATE PRODUTO SET TIPO = ? WHERE IDPRODUTO = ? ''', (descricao, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
            if cor:
                cursor.execute('''UPDATE PRODUTO SET COR = ? WHERE IDPRODUTO = ? ''', (cor, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
            if turbo:
                cursor.execute('''UPDATE PRODUTO SET TURBO = ? WHERE IDPRODUTO = ? ''', (turbo, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
            if portas:
                cursor.execute('''UPDATE PRODUTO SET PORTAS = ? WHERE IDPRODUTO = ? ''', (portas, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
            if preco:
                cursor.execute('''UPDATE PRODUTO SET PREÇO = ? WHERE IDPRODUTO = ? ''', (preco, idproduto)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_produto()
        else:
            messagebox.showinfo('ID PRODUTO', 'O PARAMETRO ID PRODUTO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_metaa = entry_datam = entry_idmeta = None


def update_meta():

    global entry_metaa, entry_datam, entry_idmeta

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Meta')
    janelaupdate.geometry('380x200')

    label_metaa = tk.Label(janelaupdate, text='VALOR DA META:')
    label_metaa.grid(row=0, column=0, padx=10, pady=10)
    entry_metaa = ttk.Entry(janelaupdate)
    entry_metaa.grid(row=0, column=1, padx=10, pady=10)

    label_datam = tk.Label(janelaupdate, text='DATA FINAL META:')
    label_datam.grid(row=1, column=0, padx=10, pady=10)
    entry_datam = ttk.Entry(janelaupdate)
    entry_datam.grid(row=1, column=1, padx=10, pady=10)

    label_idmeta = tk.Label(janelaupdate, text='PARAMETRO ID META:', bg='green', fg='white')
    label_idmeta.grid(row=2, column=0, padx=10, pady=10)
    entry_idmeta = ttk.Entry(janelaupdate)
    entry_idmeta.grid(row=2, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=2, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Produto', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_metas(entry_metaa.get(), entry_datam.get(), entry_idmeta.get()))
    botao_saida_material.grid(row=3, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_metas(valormeta, datafinal, idmeta):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idmeta:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM META_VENDEDOR WHERE IDMETA = ?''', idmeta)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID META', 'ID META não encontrado, verifique e tente novamente!')
            if valormeta:
                cursor.execute('''UPDATE META_VENDEDOR SET VALOR_META = ? WHERE IDMETA = ? ''', (valormeta, idmeta)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_meta()
            if datafinal:
                cursor.execute('''UPDATE META_VENDEDOR SET DATA_FINAL_META = ? WHERE IDMETA = ? ''', (datafinal, idmeta)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_meta()
        else:
            messagebox.showinfo('ID META', 'O PARAMETRO ID META não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_nomefor = entry_tipofor = entry_cnpjfor = entry_dddforn = entry_telefoneforn = entry_emailforn = entry_nforn = None
entry_ruaforn = entry_bairroforn = entry_cidadeforn = entry_estadoforn = entry_situacaoforn = entry_idfornecedorn = None


def update_fornecedor():

    global entry_nomefor, entry_tipofor, entry_cnpjfor, entry_dddforn, entry_telefoneforn, entry_emailforn, entry_nforn
    global entry_ruaforn, entry_bairroforn, entry_cidadeforn, entry_estadoforn, entry_situacaoforn, entry_idfornecedorn

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Fornecedor')
    janelaupdate.geometry('400x600')

    label_nomefor = tk.Label(janelaupdate, text='NOME:')
    label_nomefor.grid(row=0, column=0, padx=10, pady=10)
    entry_nomefor = ttk.Entry(janelaupdate)
    entry_nomefor.grid(row=0, column=1, padx=10, pady=10)

    label_tipofor = tk.Label(janelaupdate, text='TIPO DE MATERIAL:')
    label_tipofor.grid(row=1, column=0, padx=10, pady=10)
    entry_tipofor = ttk.Entry(janelaupdate)
    entry_tipofor.grid(row=1, column=1, padx=10, pady=10)

    label_cnpjfor = tk.Label(janelaupdate, text='CNPJ:')
    label_cnpjfor.grid(row=2, column=0, padx=10, pady=10)
    entry_cnpjfor = ttk.Entry(janelaupdate)
    entry_cnpjfor.grid(row=2, column=1, padx=10, pady=10)

    label_dddfor = tk.Label(janelaupdate, text='DDD:')
    label_dddfor.grid(row=3, column=0, padx=10, pady=10)
    entry_dddforn = ttk.Entry(janelaupdate)
    entry_dddforn.grid(row=3, column=1, padx=10, pady=10)

    label_telefonefor = tk.Label(janelaupdate, text='TELEFONE:')
    label_telefonefor.grid(row=4, column=0, padx=10, pady=10)
    entry_telefoneforn = ttk.Entry(janelaupdate)
    entry_telefoneforn.grid(row=4, column=1, padx=10, pady=10)

    label_emailfor = tk.Label(janelaupdate, text='E-MAIL:')
    label_emailfor.grid(row=5, column=0, padx=10, pady=10)
    entry_emailforn = ttk.Entry(janelaupdate)
    entry_emailforn.grid(row=5, column=1, padx=10, pady=10)

    label_nfor = tk.Label(janelaupdate, text='Nº:')
    label_nfor.grid(row=6, column=0, padx=10, pady=10)
    entry_nforn = ttk.Entry(janelaupdate)
    entry_nforn.grid(row=6, column=1, padx=10, pady=10)

    label_ruafor = tk.Label(janelaupdate, text='RUA:')
    label_ruafor.grid(row=7, column=0, padx=10, pady=10)
    entry_ruaforn = ttk.Entry(janelaupdate)
    entry_ruaforn.grid(row=7, column=1, padx=10, pady=10)

    label_bairrofor = tk.Label(janelaupdate, text='BAIRRO:')
    label_bairrofor.grid(row=8, column=0, padx=10, pady=10)
    entry_bairroforn = ttk.Entry(janelaupdate)
    entry_bairroforn.grid(row=8, column=1, padx=10, pady=10)

    label_cidadefor = tk.Label(janelaupdate, text='CIDADE:')
    label_cidadefor.grid(row=9, column=0, padx=10, pady=10)
    entry_cidadeforn = ttk.Entry(janelaupdate)
    entry_cidadeforn.grid(row=9, column=1, padx=10, pady=10)

    label_estadofor = tk.Label(janelaupdate, text='ESTADO:')
    label_estadofor.grid(row=10, column=0, padx=10, pady=10)
    entry_estadoforn = ttk.Entry(janelaupdate)
    entry_estadoforn.grid(row=10, column=1, padx=10, pady=10)

    label_situacaofor = tk.Label(janelaupdate, text='SITUAÇÃO:')
    label_situacaofor.grid(row=11, column=0, padx=10, pady=10)
    entry_situacaoforn = ttk.Entry(janelaupdate)
    entry_situacaoforn.grid(row=11, column=1, padx=10, pady=10)

    label_idfornecedor = tk.Label(janelaupdate, text='PARAMETRO ID FORNECEDOR:', bg='green', fg='white')
    label_idfornecedor.grid(row=12, column=0, padx=10, pady=10)
    entry_idfornecedorn = ttk.Entry(janelaupdate)
    entry_idfornecedorn.grid(row=12, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=12, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Fornecedor', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_fornecedores(entry_nomefor.get(), entry_tipofor.get(), entry_cnpjfor.get(), entry_dddforn.get(), entry_telefoneforn.get(), entry_emailforn.get(), entry_nforn.get(), entry_ruaforn.get(), entry_bairroforn.get(), entry_cidadeforn.get(), entry_estadoforn.get(), entry_situacaoforn.get(), entry_idfornecedorn.get()))
    botao_saida_material.grid(row=13, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_fornecedores(nome, tipo, cnpj, ddd, telefone, email, numero, rua, bairro, cidade, estado, situacao, idfornecedor):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idfornecedor:  # Verifica se o CPF não está vazio
            cursor.execute('''SELECT 1 FROM FORNECEDOR WHERE IDFORNECEDOR = ?''', idfornecedor)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID FORNECEDOR', 'ID FORNECEDOR não encontrado, verifique e tente novamente!')
            if nome:
                cursor.execute('''UPDATE FORNECEDOR SET NOME = ? WHERE IDFORNECEDOR = ? ''', (nome, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if tipo:
                cursor.execute('''UPDATE FORNECEDOR SET TIPO_DE_MATERIAL = ? WHERE IDFORNECEDOR = ? ''', (tipo, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if cnpj:
                cursor.execute('''UPDATE FORNECEDOR SET CNPJ = ? WHERE IDFORNECEDOR = ? ''', (cnpj, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if ddd:
                cursor.execute('''UPDATE FORNECEDOR SET DDD = ? WHERE IDFORNECEDOR = ? ''', (ddd, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if telefone:
                cursor.execute('''UPDATE FORNECEDOR SET TELEFONE = ? WHERE IDFORNECEDOR = ? ''', (telefone, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if email:
                cursor.execute('''UPDATE FORNECEDOR SET EMAIL = ? WHERE IDFORNECEDOR = ? ''', (email, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if numero:
                cursor.execute('''UPDATE FORNECEDOR SET NUMERO = ? WHERE IDFORNECEDOR = ? ''', (numero, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if rua:
                cursor.execute('''UPDATE FORNECEDOR SET RUA = ? WHERE IDFORNECEDOR = ? ''', (rua, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if bairro:
                cursor.execute('''UPDATE FORNECEDOR SET BAIRRO = ? WHERE IDFORNECEDOR = ? ''', (bairro, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if cidade:
                cursor.execute('''UPDATE FORNECEDOR SET CIDADE = ? WHERE IDFORNECEDOR = ? ''', (cidade, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if estado:
                cursor.execute('''UPDATE FORNECEDOR SET ESTADO = ? WHERE IDFORNECEDOR = ? ''', (estado, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
            if situacao:
                cursor.execute('''UPDATE FORNECEDOR SET SITUAÇAO = ? WHERE IDFORNECEDOR = ? ''', (situacao, idfornecedor)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_tela_fornecedor()
        else:
            messagebox.showinfo('ID FORNECEDOR', 'O PARAMETRO ID FORNECEDOR não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


entry_id_fornecedor_estoque = entry_loteest =  entry_nomeest =  entry_tipoest =  entry_corest = entry_tamanhoest = None
entry_qtdest = entry_dataest = entry_idestoque = None


def update_estoque():

    global entry_id_fornecedor_estoque, entry_loteest, entry_nomeest, entry_tipoest, entry_corest, entry_tamanhoest
    global entry_qtdest, entry_dataest, entry_idestoque

    janelaupdate = tk.Tk()
    janelaupdate.title('Atualizar Estoque')
    janelaupdate.geometry('400x450')

    label_id_fornecedor_estoque = tk.Label(janelaupdate, text='ID FORNECEDOR:')
    label_id_fornecedor_estoque.grid(row=0, column=0, padx=10, pady=10)
    entry_id_fornecedor_estoque = ttk.Entry(janelaupdate)
    entry_id_fornecedor_estoque.grid(row=0, column=1, padx=10, pady=10)

    label_loteest = tk.Label(janelaupdate, text='LOTE:')
    label_loteest.grid(row=1, column=0, padx=10, pady=10)
    entry_loteest = ttk.Entry(janelaupdate)
    entry_loteest.grid(row=1, column=1, padx=10, pady=10)

    label_nomeest = tk.Label(janelaupdate, text='NOME:')
    label_nomeest.grid(row=2, column=0, padx=10, pady=10)
    entry_nomeest = ttk.Entry(janelaupdate)
    entry_nomeest.grid(row=2, column=1, padx=10, pady=10)

    label_tipoest = tk.Label(janelaupdate, text='TIPO:')
    label_tipoest.grid(row=3, column=0, padx=10, pady=10)
    entry_tipoest = ttk.Entry(janelaupdate)
    entry_tipoest.grid(row=3, column=1, padx=10, pady=10)

    label_corest = tk.Label(janelaupdate, text='COR:')
    label_corest.grid(row=4, column=0, padx=10, pady=10)
    entry_corest = ttk.Entry(janelaupdate)
    entry_corest.grid(row=4, column=1, padx=10, pady=10)

    label_tamanhoest = tk.Label(janelaupdate, text='TAMANHO:')
    label_tamanhoest.grid(row=5, column=0, padx=10, pady=10)
    entry_tamanhoest = ttk.Entry(janelaupdate)
    entry_tamanhoest.grid(row=5, column=1, padx=10, pady=10)

    label_qtdest = tk.Label(janelaupdate, text='QUANTIDADE:')
    label_qtdest.grid(row=6, column=0, padx=10, pady=10)
    entry_qtdest = ttk.Entry(janelaupdate)
    entry_qtdest.grid(row=6, column=1, padx=10, pady=10)

    label_dataest = tk.Label(janelaupdate, text='DATA DE VALIDADE:')
    label_dataest.grid(row=7, column=0, padx=10, pady=10)
    entry_dataest = ttk.Entry(janelaupdate)
    entry_dataest.grid(row=7, column=1, padx=10, pady=10)

    label_idestoque = tk.Label(janelaupdate, text='PARAMETRO ID ESTOQUE:', bg='green', fg='white')
    label_idestoque.grid(row=8, column=0, padx=10, pady=10)
    entry_idestoque = ttk.Entry(janelaupdate)
    entry_idestoque.grid(row=8, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=8, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Atualizar Estoque', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: update_estoques(entry_id_fornecedor_estoque.get(), entry_loteest.get(), entry_nomeest.get(), entry_tipoest.get(), entry_corest.get(), entry_tamanhoest.get(), entry_qtdest.get(), entry_dataest.get(), entry_idestoque.get()))
    botao_saida_material.grid(row=9, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def update_estoques(id_fornecedor, lote, nome, tipo, cor, tamanho, quantidade, datavalidade, idestoque):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idestoque or id_fornecedor:
            cursor.execute('''SELECT 1 FROM ESTOQUE_DE_MATERIAL WHERE IDESTOQUE = ?''', idestoque)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID ESTOQUE', 'ID ESTOQUE não encontrado, verifique e tente novamente!')
            cursor.execute('''SELECT 1 FROM FORNECEDOR WHERE IDFORNECEDOR = ?''', id_fornecedor)
            resultado2 = cursor.fetchone()
            if not resultado2:
                return messagebox.showwarning('ID FORNECEDOR', 'ID FORNECEDOR não encontrado, verifique e tente novamente!')
            else:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET ID_FORNECEDOR = ? WHERE IDESTOQUE = ? ''', (id_fornecedor, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if lote:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET LOTE = ? WHERE IDESTOQUE = ? ''', (lote, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if nome:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET NOME = ? WHERE IDESTOQUE = ? ''', (nome, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if tipo:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET TIPO = ? WHERE IDESTOQUE = ? ''', (tipo, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if cor:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET COR = ? WHERE IDESTOQUE = ? ''', (cor, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if tamanho:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET TAMANHO = ? WHERE IDESTOQUE = ? ''', (tamanho, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if quantidade:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET QUANTIDADE = ? WHERE IDESTOQUE = ? ''', (quantidade, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
            if datavalidade:
                cursor.execute('''UPDATE ESTOQUE_DE_MATERIAL SET DATA_DE_VALIDADE = ? WHERE IDESTOQUE = ? ''', (datavalidade, idestoque)) # O ERRO ESTA EM NAO COLOCAR ()) NOS PARAMETROS
                conn.commit()
                messagebox.showinfo('Sucesso', 'Atualização realizada!')
                limpar_tela_update_estoque()
        else:
            messagebox.showinfo('ID ESTOQUE', 'O PARAMETRO ID ESTOQUE não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível Atualizar! {erro}')
    finally:
        cursor.close()
        conn.close()


# ############################################## DELETES ################################################################

def delete_cliente():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR CLIENTE')
    janelaupdate.geometry('380x180')

    label_idcliente = tk.Label(janelaupdate, text='ID CLIENTE:', bg='green')
    label_idcliente.grid(row=0, column=0, padx=10, pady=10)
    entry_idcliente = ttk.Entry(janelaupdate)
    entry_idcliente.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Cliente', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_clientes(entry_idcliente.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_clientes(idcliente):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idcliente:  # Verifica se o CPF não está vazio
            cursor.execute('''DELETE FROM CLIENTE WHERE IDCLIENTE = ?''', idcliente)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID VENDEDOR', 'O ID CLIENTE não encontrado, verifique e tente novamente!')
        else:
            messagebox.showinfo('ID CLIENTE', 'O PARAMETRO ID VENDEDOR não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_funcionario():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR FUNCIONÁRIO')
    janelaupdate.geometry('380x180')

    label_idfuncionario = tk.Label(janelaupdate, text='ID FUNCIONÁRIO:', bg='green')
    label_idfuncionario.grid(row=0, column=0, padx=10, pady=10)
    entry_idfuncionario = ttk.Entry(janelaupdate)
    entry_idfuncionario.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Funcionário', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_funcionarios(entry_idfuncionario.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_funcionarios(idfuncionario):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idfuncionario:  # Verifica se o CPF não está vazio
            cursor.execute('''DELETE FROM FUNCIONARIO WHERE IDFUNCIONARIO = ?''', idfuncionario)
            resultado = cursor.fetchone()
            if not resultado:
                return messagebox.showwarning('ID VENDEDOR', 'O ID CLIENTE não encontrado, verifique e tente novamente!')
        else:
            messagebox.showinfo('ID FUNCIONARIO', 'O PARAMETRO ID FUNCIONARIO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_cargo():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR CARGOS')
    janelaupdate.geometry('380x180')

    label_idcargo = tk.Label(janelaupdate, text='ID CARGO :', bg='green')
    label_idcargo.grid(row=0, column=0, padx=10, pady=10)
    entry_idcargo = ttk.Entry(janelaupdate)
    entry_idcargo.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Cargo', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_cargos(entry_idcargo.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_cargos(idcargo):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idcargo:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM CARGO WHERE IDCARGO = ?''', idcargo)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID CARGO', 'O ID CARGO não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID CARGO', 'O PARAMETRO ID CARGO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_meta():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR METAS')
    janelaupdate.geometry('380x180')

    label_idemta = tk.Label(janelaupdate, text='ID META :', bg='green')
    label_idemta.grid(row=0, column=0, padx=10, pady=10)
    entry_idmeta = ttk.Entry(janelaupdate)
    entry_idmeta.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Meta', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_metas(entry_idmeta.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_metas(idmeta):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idmeta:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM META_VENDEDOR WHERE IDMETA = ?''', idmeta)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID META', 'O ID META não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID META', 'O PARAMETRO ID META não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_produto():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR PRODUTO')
    janelaupdate.geometry('380x180')

    label_idproduto = tk.Label(janelaupdate, text='ID PRODUTO :', bg='green')
    label_idproduto.grid(row=0, column=0, padx=10, pady=10)
    entry_idproduto = ttk.Entry(janelaupdate)
    entry_idproduto.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Produto', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_produtos(entry_idproduto.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_produtos(idproduto):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idproduto:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM PRODUTO WHERE IDPRODUTO = ?''', idproduto)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID PRODUTO', 'O ID PRODUTO não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID PRODUTO', 'O PARAMETRO ID PRODUTO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_fabricacao():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR FABRICAÇÃO')
    janelaupdate.geometry('380x180')

    label_idfabricacao = tk.Label(janelaupdate, text='ID FABRICAÇÃO :', bg='green')
    label_idfabricacao.grid(row=0, column=0, padx=10, pady=10)
    entry_idfabricacao = ttk.Entry(janelaupdate)
    entry_idfabricacao.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Fabricação', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_fabricacoes(entry_idfabricacao.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_fabricacoes(idfabricacao):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idfabricacao:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM PRODUTO_EM_FABRICAÇAO WHERE IDFABRICAÇAO = ?''', idfabricacao)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID FABRICAÇÃO', 'O ID FABRICAÇÃO não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID FABRICAÇÃO', 'O PARAMETRO ID FABRICAÇÃO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_fornecedor():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR FORNECEDOR')
    janelaupdate.geometry('380x180')

    label_idfornecedor = tk.Label(janelaupdate, text='ID FORNECEDOR :', bg='green')
    label_idfornecedor.grid(row=0, column=0, padx=10, pady=10)
    entry_idfornecedor= ttk.Entry(janelaupdate)
    entry_idfornecedor.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Fornecedor', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_fornecedores(entry_idfornecedor.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_fornecedores(idfornecedor):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idfornecedor:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM FORNECEDOR WHERE IDFORNECEDOR = ?''', idfornecedor)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID FORNECEDOR', 'O ID FORNECEDOR não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID FORNECEDOR', 'O PARAMETRO ID FORNECEDOR não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_nota():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR NOTA FISCAL')
    janelaupdate.geometry('380x180')

    label_idnota = tk.Label(janelaupdate, text='ID NOTA :', bg='green')
    label_idnota.grid(row=0, column=0, padx=10, pady=10)
    entry_idnota= ttk.Entry(janelaupdate)
    entry_idnota.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Nota Fiscal', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_notas(entry_idnota.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_notas(idnota):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idnota:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM NF_CLIENTE WHERE IDNF = ?''', idnota)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID NOTA FISCAL', 'O ID NOTA FISCAL não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID NOTA FISCAL', 'O PARAMETRO ID NOTA FISCAL não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_saida():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR SAIDA')
    janelaupdate.geometry('380x180')

    label_idmaterial = tk.Label(janelaupdate, text='ID MATERIAL :', bg='green')
    label_idmaterial.grid(row=0, column=0, padx=10, pady=10)
    entry_idmaterial = ttk.Entry(janelaupdate)
    entry_idmaterial.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Material', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_saidas(entry_idmaterial.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_saidas(idmaterial):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idmaterial:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM MATERIAL_PRA_CONSUMO WHERE IDMATERIAL = ?''', idmaterial)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID MATERIAL', 'O ID MATERIAL não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID MATERIAL', 'O PARAMETRO ID MATERIAL não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


def delete_produto_fabricacao():
    janelaupdate = tk.Tk()
    janelaupdate.title('EXCLUIR PRODUÇÃO')
    janelaupdate.geometry('380x180')

    label_idproducao = tk.Label(janelaupdate, text='ID FABRICAÇÃO :', bg='green')
    label_idproducao.grid(row=0, column=0, padx=10, pady=10)
    entry_idproducao = ttk.Entry(janelaupdate)
    entry_idproducao.grid(row=0, column=1, padx=10, pady=10)

    desc_cpfup = tk.Label(janelaupdate, text='*')
    desc_cpfup.grid(row=0, column=2, sticky="w")

    botao_saida_material = tk.Button(janelaupdate, text='Excluir Produção', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: delete_produto_fabricacoes(entry_idproducao.get()))
    botao_saida_material.grid(row=1, column=1, padx=10, pady=10)

    janelaupdate.mainloop()


def delete_produto_fabricacoes(idproducao):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        if idproducao:  # Verifica se o ID CARGO não está vazio, SE NAO ESTIVER, VAI PRO CURSOR
            cursor.execute('''DELETE FROM PRODUTO_EM_FABRICAÇAO WHERE IDFABRICAÇAO = ?''', idproducao)
            resultado = cursor.fetchone()
            if not resultado: # SE NAO DELETOU NADA, PQ NAO ACHOU O ID CARGO ELE VAI DAR A MENSAGEM ABAIXO
                return messagebox.showwarning('ID FABRICAÇÃO', 'O ID FABRICAÇÃO não encontrado, verifique e tente novamente!')
        else: # SE O ID CARGO ESTIVER VAZIO
            messagebox.showinfo('ID FABRICAÇÃO', 'O PARAMETRO ID FABRICAÇÃO não pode ficar vazio!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível deletar! {erro}')
    finally:
        cursor.close()
        conn.close()


# ########################################### PROCEDURE ################################################################

def cadastro_nf():
    janelaconsumo = tk.Tk()
    janelaconsumo.title('NF')
    janelaconsumo.geometry('300x100')

    label_id_pedido = tk.Label(janelaconsumo, text='ID PEDIDO:')
    label_id_pedido.grid(row=0, column=0, padx=10, pady=10)
    entry_id_pedido = ttk.Entry(janelaconsumo)
    entry_id_pedido.grid(row=0, column=1, padx=10, pady=10)

    botao_saida_material = tk.Button(janelaconsumo, text='Gerar Nota Fiscal', bg='blue', fg='white', bd=3, takefocus=False, font=('Helvetica', 10, 'bold'), command=lambda: inserir_dados_nf(entry_id_pedido.get()))
    botao_saida_material.grid(row=1, column=0, padx=10, pady=10)

    janelaconsumo.mainloop()


def inserir_dados_nf(idpedido):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not idpedido:
            messagebox.showwarning('Campos Vazios', 'Por favor, preencha todos os campos!')
            return

        # Verificando se o pedido existe
        cursor.execute('''SELECT idpedido FROM PEDIDO WHERE idpedido = ?''', idpedido)
        resultado = cursor.fetchone()
        if not resultado:
            messagebox.showwarning('PEDIDO', 'Pedido não encontrado, verifique e tente novamente!')
            return

        # Verificando se já existe uma nota fiscal para o pedido
        cursor.execute('''SELECT id_pedido FROM NF_CLIENTE WHERE ID_PEDIDO = ?''', idpedido)
        resultado2 = cursor.fetchone()
        if resultado2:
            messagebox.showerror('Nota já Gerada', 'Esse pedido já tem uma nota fiscal gerada! Para gerar outra, exclua a nota já gerada!')
            return
        else:
            # Passando o idpedido como parâmetro para o procedimento armazenado
            cursor.execute('EXEC SP_GERA_NF_CLIENTE ?', (idpedido,))
            conn.commit()
            messagebox.showinfo('Sucesso', 'Nota Fiscal foi gerada com sucesso!')
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possível gerar NF {erro}')


def criar_janela_exibicao_nf(dados):
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('NF')
    janelaexibicao.geometry('1350x500')

    def exibir_dados_no_treeview(filtro=None):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        dados_filtrados = []

        if filtro is not None:
            for resultado in dados:
                valores = resultado.split(', ')

                if str(filtro).upper() == valores[10].upper():
                    valores_upper = [valor.upper() for valor in valores]
                    treeview.insert('', 'end', values=valores_upper)
                    dados_filtrados.append(resultado)

        total_nf_filtro = len(dados_filtrados)
        label_total_nf['text'] = f'Total de Notas: {total_nf}                     (Com Filtro: {total_nf_filtro})'

        if not dados_filtrados and filtro is not None:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Caixa de entrada (Entry) para o filtro
    entry_filtro = tk.Entry(janelaexibicao, width=10)
    entry_filtro.pack(side='top', pady=5)

    # Botão para acionar a filtragem
    button_filtrar = tk.Button(janelaexibicao, text='Filtrar Por:  (ID PEDIDO)', bg='black', fg='white', bd=3, font=('Arial', 8, 'bold'), command=lambda: exibir_dados_no_treeview(entry_filtro.get()))
    button_filtrar.pack(side='top', pady=5)

    # Realiza a consulta COUNT(*) antes de criar a janela
    conn = conectar_banco()
    cursor = conn.cursor()
    cursor.execute('SELECT COUNT(*) FROM NF_CLIENTE')
    total_nf = cursor.fetchone()[0]

    # Adiciona rótulo para mostrar o total de áreas
    label_total_nf = tk.Label(janelaexibicao, text=f'Total de Produtos: {total_nf}', font=('Arial', 12, 'bold'))
    label_total_nf.pack(side='top', padx=10, pady=5)

    # Cria o Treeview com as colunas
    colunas = ('ID NF', 'EMISSÃO', ' NOME', 'CPF', 'QUANTIDADE', 'CNPJ FABRICANTE', 'CNPJ', 'PREÇO TOTAL', 'PROTOCOLO', 'ID FABRICAÇÃO', 'ID PEDIDO', 'NOME PRODUTO', 'TIPO', 'COR', 'TURBO', 'PORTAS')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [5, 40, 70, 50, 8, 65, 60, 30, 85, 8, 8, 60, 8, 8, 8, 8]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Chama a função para exibir os dados no Treeview (sem filtro inicial)
    exibir_dados_no_treeview()

    janelaexibicao.mainloop()


def selecionar_dados_nf():
    try:
        conn = conectar_banco()
        cursor = conn.cursor()
        cursor.execute('''SELECT * FROM NF_CLIENTE''')
        resultados = []

        for linha in cursor.fetchall():
            idnf, data, nome, cpf, quantidade, cnpjf, cnpj, preco, protocolo, idfabricacao, idpedido, nomep, tipo, cor, turbo, portas = linha
            resultado_str = f'{idnf}, {data}, {nome}, {cpf}, {quantidade}, {cnpjf}, {cnpj}, {round(preco, 2)}, {protocolo}, {idfabricacao}, {idpedido}, {nomep}, {tipo}, {cor}, {turbo}, {portas} ' # NÃO ESQUÇAO DO ESPAÇO AQUI.
            resultados.append(resultado_str)

        # Chama a função para criar e exibir a nova janela
        criar_janela_exibicao_nf(resultados) # chamar a def que cria a janela de exibição

    except pyodbc.Error as erro:
        messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')


def criar_janela_exibicao_material_pedido():
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Material Por Pedido')
    janelaexibicao.geometry('1350x500')

    # Cria o Treeview com as colunas
    colunas = ('ID FORNECEDOR', 'LOTE', ' NOME', 'TIPO', 'COR', 'TAMANHO', 'DATA DE VALIDADE', 'DATA SAÍDA', 'ID PEDIDO', 'ID ESTOQUE', 'QUANTIDADE')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [5, 40, 130, 50, 10, 20, 60, 45, 10, 8, 8]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Campo de entrada para o ID do pedido
    entry_idpedido = tk.Entry(janelaexibicao, width=40, bg='gray')
    entry_idpedido.pack(pady=10)

    def selecionar_dados_material_pedido():
        try:
            conn = conectar_banco()
            cursor = conn.cursor()

            # Obtém o valor do campo de entrada
            idpedido = entry_idpedido.get()

            # Ajuste para lidar com o argumento idpedido opcional
            if not idpedido:
                messagebox.showwarning('Campos vazios', 'Preencha todos os dados!')
                return  # Saia da função se idpedido estiver vazio

                # Executa a consulta
            cursor.execute('EXEC SP_CONSULTA_MATERIAL ?', idpedido)

            # Obtém os resultados
            resultados = []

            for linha in cursor.fetchall():
                idfornecedor, lote, nome, tipo, cor, tamanho, datavalidade, datasaida, id_pedido, idestoque, quantidade = linha
                resultado_str = f'{idfornecedor}, {lote}, {nome}, {tipo}, {cor}, {tamanho}, {datavalidade}, {datasaida}, {id_pedido}, {idestoque}, {quantidade}'
                resultados.append(resultado_str)

            # Exibe os resultados no Treeview
            exibir_dados_no_treeview(resultados)

            # Verifica se há resultados
            if not resultados:
                messagebox.showwarning('Vazio', 'Não há resultados para a sua consulta!')

        except pyodbc.Error as erro:
            messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')

    def exibir_dados_no_treeview(dados):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        for resultado in dados:
            treeview.insert('', 'end', values=resultado.split(', '))

        if not dados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Botão para acionar a consulta
    botao_consultar = tk.Button(janelaexibicao, text='Consultar', bg='black', fg='white', takefocus=False, bd=7, command=selecionar_dados_material_pedido)
    botao_consultar.pack(pady=10)

    janelaexibicao.mainloop()


def criar_janela_exibicao_sp_pagamento():
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Consulta Pagamento por CPF')
    janelaexibicao.geometry('1000x500')

    # Cria o Treeview com as colunas
    colunas = ('DATA DE EMISSÃO', 'NOME', ' CPF', 'PIS', 'CARGO', 'DATA DO PAGAMENTO', 'MES DE REFERÊNCIA', 'SALÁRIO BRUTO', 'INSS', 'SALÁRIO LIQUIDO', 'EMPRESA', 'CNPJ')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [50, 100, 55, 70, 50, 95, 80, 60, 20, 60, 45, 80]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Campo de entrada para o ID do pedido
    entry_cpf = tk.Entry(janelaexibicao, width=40, bg='gray')
    entry_cpf.pack(pady=10)

    def selecionar_dados_sp_pagamento():
        try:
            conn = conectar_banco()
            cursor = conn.cursor()

            # Obtém o valor do campo de entrada
            idcpf = entry_cpf.get()

            # Ajuste para lidar com o argumento idpedido opcional
            if not idcpf:
                messagebox.showwarning('Campos vazios', 'Preencha todos os dados!')
                return  # Saia da função se idpedido estiver vazio

                # Executa a consulta
            cursor.execute('exec sp_consulta_pagamento ?',  idcpf)

            # Obtém os resultados
            resultados = []

            for linha in cursor.fetchall():
                dataemissao, nome, cpf, pis, cargo, datapagamento, mes, salariobruto, inss, salarioliquido, empresa, cnpj = linha
                resultado_str = f'{dataemissao}, {nome}, {cpf}, {pis}, {cargo}, {datapagamento}, {mes}, {salariobruto}, {inss}, {salarioliquido}, {empresa}, {cnpj}'
                resultados.append(resultado_str)

            # Exibe os resultados no Treeview
            exibir_dados_no_treeview(resultados)

            # Verifica se há resultados
            if not resultados:
                messagebox.showwarning('Vazio', 'Não há resultados para a sua consulta!')

        except pyodbc.Error as erro:
            messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')

    def exibir_dados_no_treeview(dados):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        for resultado in dados:
            treeview.insert('', 'end', values=resultado.split(', '))

        if not dados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Botão para acionar a consulta
    botao_consultar = tk.Button(janelaexibicao, text='Consultar', bg='black', fg='white', takefocus=False, bd=7, command=selecionar_dados_sp_pagamento)
    botao_consultar.pack(pady=10)

    janelaexibicao.mainloop()


def criar_janela_exibicao_info_vendedor():
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Vendedores')
    janelaexibicao.geometry('600x500')

    # Cria o Treeview com as colunas
    colunas = ('NOME', 'CPF', ' CIDADE', 'ESTADO', 'DDD', 'TELEFONE', 'ID VENDEDOR')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [60, 60, 60, 20, 20, 60, 20]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Campo de entrada para o ID do Vendedor
    entry_idvendedor = tk.Entry(janelaexibicao, width=40, bg='gray')
    entry_idvendedor.pack(pady=10)

    def selecionar_dados_info_vendedor():
        try:
            conn = conectar_banco()
            cursor = conn.cursor()

            # Obtém o valor do campo de entrada
            idvendedor = entry_idvendedor.get()

            # Ajuste para lidar com o argumento idpedido opcional
            if not idvendedor:
                messagebox.showwarning('Campos vazios', 'Preencha todos os dados!')
                return  # Saia da função se idpedido estiver vazio

                # Executa a consulta
            cursor.execute('EXEC SP_INFO_VENDEDOR ?', idvendedor)

            # Obtém os resultados
            resultados = []

            for linha in cursor.fetchall():
                nome, cpf, cidade, estado, ddd, telefone, idvendedor = linha
                resultado_str = f'{nome}, {cpf}, {cidade}, {estado}, {ddd}, {telefone}, {idvendedor}'
                resultados.append(resultado_str)

            # Exibe os resultados no Treeview
            exibir_dados_no_treeview(resultados)

            # Verifica se há resultados
            if not resultados:
                messagebox.showwarning('Vazio', 'Não há resultados para a sua consulta!')

        except pyodbc.Error as erro:
            messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')

    def exibir_dados_no_treeview(dados):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        for resultado in dados:
            treeview.insert('', 'end', values=resultado.split(', '))

        if not dados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Botão para acionar a consulta
    botao_consultar = tk.Button(janelaexibicao, text='Consultar', bg='black', fg='white', takefocus=False, bd=7, command=selecionar_dados_info_vendedor)
    botao_consultar.pack(pady=10)

    janelaexibicao.mainloop()


def criar_janela_exibicao_status_pedido():
    # Cria a janela
    janelaexibicao = tk.Tk()
    janelaexibicao.title('Status Pedido')
    janelaexibicao.geometry('800x500')

    # Cria o Treeview com as colunas
    colunas = ('NOME', 'CPF', ' CNPJ', 'PRODUTO', 'ID PEDIDO', 'DATA DE EMISSÃO', 'DATA DE ENTREGA', 'PREÇO TOTAL', 'ID VENDEDOR', 'STATUS')
    treeview = ttk.Treeview(janelaexibicao, columns=colunas, show='headings')

    # Configura as colunas com larguras menores
    larguras = [60, 60, 90, 80, 20, 60, 60, 60, 20, 40]
    for coluna, largura in zip(colunas, larguras):
        treeview.heading(coluna, text=coluna)
        treeview.column(coluna, width=largura, anchor='center')

    # Adiciona o Treeview à janela
    treeview.pack(expand=True, fill='both')

    # Label para mostrar mensagem quando não houver resultados
    label_sem_resultados = tk.Label(janelaexibicao, text='', fg='red')

    # Campo de entrada para o ID do Vendedor
    entry_idvendedor = tk.Entry(janelaexibicao, width=40, bg='gray')
    entry_idvendedor.pack(pady=10)

    def selecionar_dados_status_pedido():
        try:
            conn = conectar_banco()
            cursor = conn.cursor()

            # Obtém o valor do campo de entrada
            consulta = entry_idvendedor.get()

            # Ajuste para lidar com o argumento idpedido opcional
            if not consulta:
                messagebox.showwarning('Campos vazios', 'Preencha todos os dados!')
                return  # Saia da função se idpedido estiver vazio

            # Executa a consulta
            cursor.execute('EXEC SP_CONSULTA_STATUS_PEDIDO ?, ?', consulta, consulta)

            # Obtém os resultados
            resultados = []

            for linha in cursor.fetchall():
                nome, cpf, cnpj, produto, idpedido, dataemissao, dataentrega, precototal, idvendedor, status = linha
                resultado_str = f'{nome}, {cpf}, {cnpj}, {produto}, {idpedido}, {dataemissao}, {dataentrega}, {precototal}, {idvendedor}, {status}'
                resultados.append(resultado_str)

            # Exibe os resultados no Treeview
            exibir_dados_no_treeview(resultados)

            # Verifica se há resultados
            if not resultados:
                messagebox.showwarning('Vazio', 'Não há resultados para a sua consulta!')

        except pyodbc.Error as erro:
            messagebox.showerror('Erro', f'Não foi possível acessar os dados no banco {erro}')
        finally:
            # Certifique-se de fechar a conexão e o cursor, mesmo em caso de erro
            cursor.close()
            conn.close()

    def exibir_dados_no_treeview(dados):
        treeview.delete(*treeview.get_children())  # Limpa os dados anteriores
        for resultado in dados:
            treeview.insert('', 'end', values=resultado.split(', '))

        if not dados:
            label_sem_resultados.pack(side='top', padx=10, pady=5, fill='both')
        else:
            label_sem_resultados.pack_forget()  # Esconde a mensagem se houver resultados

    # Botão para acionar a consulta
    botao_consultar = tk.Button(janelaexibicao, text='Consultar', bg='black', fg='white', takefocus=False, bd=7, command=selecionar_dados_status_pedido)
    botao_consultar.pack(pady=10)

    janelaexibicao.mainloop()


# ################################################ CRIAÇÃO DE LOGIN E VERIFICAÇÃO ######################################


def criar_janela_login():
    notebook.hide(aba_funcionario)
    notebook.hide(aba_administrativo)
    notebook.hide(aba_comercial)
    notebook.hide(aba_cliente)

    def validar_login(usuario, senha):
        try:
            conn = conectar_banco()
            if conn:
                cursor = conn.cursor()

                hashed_senha = hashlib.sha256(senha.encode()).hexdigest()
                cursor.execute("SELECT 1 FROM USUARIOS WHERE LOGIN = ? AND SENHA = ?", (usuario.lower(), hashed_senha))

                # Verifica se o resultado da consulta é não nulo
                result = cursor.fetchone()
                cursor.close()
                conn.commit()  # Confirma as alterações no banco de dados

                return result is not None
        except pyodbc.Error as erro:
            messagebox.showerror('ERRO', f'Não foi possível se conectar')

    def fazer_login():
        usuario = usuario_entry.get()
        senha = senha_entry.get()

        if usuario and senha:
            if validar_login(usuario, senha):
                messagebox.showinfo("Sucesso", "Login bem-sucedido.")
                login_window.withdraw()  # Oculta a janela de login
                notebook.add(aba_funcionario)
                notebook.add(aba_administrativo)
                notebook.add(aba_comercial)
                notebook.add(aba_cliente)
            else:
                messagebox.showwarning("Aviso", "Usuário ou senha incorretos.")
        else:
            messagebox.showwarning("Aviso", "Por favor, preencha todos os campos.")

    # Criação da janela principal
    root = tk.Tk()
    root.withdraw()

    # Estilo para o frame (cores e fontes)
    style = ttk.Style()
    style.configure('TLabel', background='#3498db', font=('Helvetica', 12))
    style.configure('TButton', background='#2ecc71', font=('Helvetica', 12))

    # Criação da janela de login
    login_window = tk.Toplevel()
    login_window.title("Login")
    login_window.geometry("700x400")  # Defina o tamanho desejado

    # Adicione um frame para conter os widgets com uma cor de fundo diferente
    frame = ttk.Frame(login_window, style='TFrame')
    frame.place(relwidth=0.8, relheight=0.8, relx=0.1, rely=0.1)

    # Adicione uma entrada de usuário
    ttk.Label(frame, text="Usuário:").pack(pady=10)
    usuario_entry = ttk.Entry(frame)
    usuario_entry.pack(pady=10)

    # Adicione uma entrada de senha
    ttk.Label(frame, text="Senha:").pack(pady=10)
    senha_entry = ttk.Entry(frame, show="*")
    senha_entry.pack(pady=10)

    # Adiciona botão para fazer login
    login_button = ttk.Button(frame, text="Login", command=fazer_login)
    login_button.pack(pady=20)

    # Mostrar a janela de login
    login_window.mainloop()


entry_login = entry_senha = None


def criar_janela_cadastro_senhageral():

    global entry_login, entry_senha

    janelalogin = tk.Tk()
    janelalogin.title('Cadastro de Login')
    janelalogin.geometry('300x170')

    label_login = tk.Label(janelalogin, text='Nome de Usuário:')
    label_login.grid(row=0, column=1, padx=10, pady=10)
    entry_login = ttk.Entry(janelalogin, style='EstiloEntry.TEntry', width=20)
    entry_login.grid(row=0, column=2, padx=10, pady=10)

    label_senha = tk.Label(janelalogin, text='Senha:')
    label_senha.grid(row=1, column=1, padx=10, pady=10)
    entry_senha = ttk.Entry(janelalogin, style='EstiloEntry.TEntry', width=20)
    entry_senha.grid(row=1, column=2, padx=10, pady=10)

    botao_inserir_login = tk.Button(janelalogin, text='Cadastrar Login', bg='blue', fg='white', bd=5, font=('Helvetica', 8, 'bold'), command=lambda: cadastrar_senhageral(entry_login.get(), entry_senha.get()))
    botao_inserir_login.grid(row=2, column=1, padx=10, pady=10)


def cadastrar_senhageral(usuario, senha):
    try:
        conn = conectar_banco()
        cursor = conn.cursor()

        if not usuario or not senha:
            messagebox.showwarning('Campos Vazios', 'Por Favor, Preencha todos os campos')
            return

        else:
            hashed_senha = hashlib.sha256(senha.encode()).hexdigest()
            cursor.execute('''INSERT INTO usuarios (LOGIN, SENHA) VALUES (?, ?)''', usuario.lower(), hashed_senha)
            conn.commit()
            messagebox.showinfo('Sucesso', 'Usuário Cadastrado com sucesso!')
            limpar_tela_cadastro_login()
    except pyodbc.Error as erro:
        return messagebox.showerror('Erro', f'Não foi possivel inseridos os dados! {erro}')


# ################################## janela principal ###################################################

janela = tk.Tk()
janela.title('FABRICAR LTDA')
# janela.geometry('700x500') # se eu conseguir deixar o logo da empresa eu aumento o tamnho

# criar o espaco para as abas
notebook = ttk.Notebook(janela)
notebook.grid(row=0, column=0, columnspan=3, rowspan=4, padx=10, pady=10)

# estilo
style = ttk.Style()
style.configure('EstiloEntry.TEntry', width=100)


# #################################################### ABAS #############################################################

# ABA CLIENTE
# criando a primeira aba CLIENTE
aba_cliente = ttk.Frame(notebook, takefocus=False)
aba_cliente.grid(row=0, column=0)

# nomeando a aba CLIENTE
notebook.add(aba_cliente, text='Cadastrar Cliente')

# criando os campos pra CLIENTE
label_nome = tk.Label(aba_cliente, text='Nome: ')
label_nome.grid(row=0, column=0, padx=10, pady=10)
entry_nome = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_nome.grid(row=0, column=1, padx=10, pady=10)

label_data_de_nascimento = tk.Label(aba_cliente, text='Data de Nascimento: ')
label_data_de_nascimento.grid(row=1, column=0, padx=10, pady=10)
entry_data_de_nascimento = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_data_de_nascimento.grid(row=1, column=1, padx=10, pady=10, sticky="w")

label_cpf = tk.Label(aba_cliente, text='Cpf:')
label_cpf.grid(row=2, column=0, padx=10, pady=10)
entry_cpf = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_cpf.grid(row=2, column=1, padx=10, pady=10, sticky="w")

label_cnpj = tk.Label(aba_cliente, text='Cnpj:')
label_cnpj.grid(row=3, column=0, padx=10, pady=10)
entry_cnpj = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_cnpj.grid(row=3, column=1, padx=10, pady=10, sticky="w")

label_email = tk.Label(aba_cliente, text='E-mail:')
label_email.grid(row=4, column=0, padx=10, pady=10)
entry_email = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_email.grid(row=4, column=1, padx=10, pady=10, sticky="w")

label_rua = tk.Label(aba_cliente, text='Rua:')
label_rua.grid(row=5, column=0, padx=10, pady=10)
entry_rua = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=23)
entry_rua.grid(row=5, column=1, padx=10, pady=10, sticky="w")

label_numero = tk.Label(aba_cliente, text='')
label_numero.grid(row=5, column=2, padx=10, pady=10)
entry_numero = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=15)
entry_numero.grid(row=5, column=1, padx=10, pady=10, sticky="e")

label_bairro = tk.Label(aba_cliente, text='Bairro:')
label_bairro.grid(row=7, column=0, padx=10, pady=10)
entry_bairro = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_bairro.grid(row=7, column=1, padx=10, pady=10, sticky="w")

label_cidade = tk.Label(aba_cliente, text='Cidade:')
label_cidade.grid(row=8, column=0, padx=10, pady=10)
entry_cidade = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_cidade.grid(row=8, column=1, padx=10, pady=10, sticky="w")

label_estado = tk.Label(aba_cliente, text='Estado:')
label_estado.grid(row=9, column=0, padx=10, pady=10)
entry_estado = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=50)
entry_estado.grid(row=9, column=1, padx=10, pady=10, sticky="w")

label_ddd = tk.Label(aba_cliente, text='DDD:')
label_ddd.grid(row=10, column=0, padx=10, pady=10)
entry_ddd = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=10)
entry_ddd.grid(row=10, column=1, padx=10, pady=10, sticky="w")

label_telefone = tk.Label(aba_cliente, text='Telefone:')
label_telefone.grid(row=10, column=1, padx=10, pady=10)
entry_telefone = ttk.Entry(aba_cliente, style='EstiloEntry.TEntry', width=13)
entry_telefone.grid(row=10, column=1, padx=10, pady=10, sticky="e")

# descriçao para os campos (obrigatorios ou opcional)
desc_nome = tk.Label(aba_cliente, text='(Obrigatório)')
desc_nome.grid(row=0, column=3, padx=5, pady=10)

desc_data = tk.Label(aba_cliente, text='(Obrigatório)')
desc_data.grid(row=1, column=3, padx=5, pady=10)

desc_cpf = tk.Label(aba_cliente, text='(Obrigatório)')
desc_cpf.grid(row=2, column=3, padx=5, pady=10)

desc_cnpj = tk.Label(aba_cliente, text='(Obrigatório)')
desc_cnpj.grid(row=3, column=3, padx=5, pady=10)

desc_email = tk.Label(aba_cliente, text='(Obrigatório)')
desc_email.grid(row=4, column=3, padx=5, pady=10)

desc_rua = tk.Label(aba_cliente, text='(Obrigatório)      Nº:')
desc_rua.grid(row=5, column=1, padx=5, pady=10)

desc_numero = tk.Label(aba_cliente, text='(Opcional)')
desc_numero.grid(row=5, column=3, padx=5, pady=10)

desc_bairro = tk.Label(aba_cliente, text='(Obrigatório)')
desc_bairro.grid(row=7, column=3, padx=5, pady=10)

desc_cidade = tk.Label(aba_cliente, text='(Obrigatório)')
desc_cidade.grid(row=8, column=3, padx=5, pady=10)

desc_estado = tk.Label(aba_cliente, text='(Obrigatório)')
desc_estado.grid(row=9, column=3, padx=5, pady=10)

desc_ddd = tk.Label(aba_cliente, text='(Obrigatório)      Telefone: ')
desc_ddd.grid(row=10, column=1, padx=5, pady=10)

desc_telefone = tk.Label(aba_cliente, text='(Obrigatório)')
desc_telefone.grid(row=10, column=3, padx=5, pady=10)

# Configurando o estilo para os botões.
style = ttk.Style()
style.configure('Estilo.TButton', foreground='blue')

# botao de inserir cliente
botao_inserir_cliente = ttk.Button(aba_cliente, text='Inserir Cadastro', command=botao_pra_inserir_cliente, style='Estilo.TButton', takefocus=False, width=25)
botao_inserir_cliente.grid(row=13, columnspan=3, padx=10, pady=10)

# ABA RECURSO HUMANOS (FUNCIONARIO) BOTOES QUE VAO FICAR NA ABA DO 'RECURSOS HUMANOS'
aba_funcionario = ttk.Frame(notebook, takefocus=False)
aba_funcionario.grid(row=0, column=1)

# nomeando a segunda aba (funcionario)
notebook.add(aba_funcionario, text='Recursos Humano')

# ABA FUNCIONÁRIOS
botao_cadastro_funcionario = tk.Button(aba_funcionario, text='Cadastrar Funcionário', command=criar_janela_cadastro, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastro_funcionario.grid(row=1, column=1, padx=10, pady=10)

botao_visualizar_funcionario = tk.Button(aba_funcionario, text='Visualizar Funcionários', command=selecionar_dados_funcionario,  bg='yellow', fg='black', width=45, takefocus=False, bd=3)
botao_visualizar_funcionario.grid(row=1, column=2, padx=10, pady=10)

botao_selecionar_cliente = tk.Button(aba_cliente, text='Visualizar Clientes', command=selecionar_dados_cliente, bg='green', fg='white', width=25, takefocus=False, bd=5)
botao_selecionar_cliente.grid(row=13, column=3, padx=10, pady=10) # ao clicar nesse botao

botao_cadastrar_cargo = tk.Button(aba_funcionario, text='Cadastrar Cargo', command=criar_janela_cadastro_cargo, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_cargo.grid(row=2, column=1, padx=10, pady=10)

botao_visualizar_cargo = tk.Button(aba_funcionario, text='Visualizar Cargos', command=selecionar_dados_cargo, bg='yellow', fg='black', width=45, takefocus=False, bd=3)
botao_visualizar_cargo.grid(row=2, column=2, padx=10, pady=10) # aqui chama os resultados quando clicado

botao_cadastrar_estado = tk.Button(aba_funcionario, text='Cadastrar Estados', command=criar_janela_cadastro_estado, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_estado.grid(row=3, column=1, padx=10, pady=10)

botao_visualizar_estado = tk.Button(aba_funcionario, text='Visualizar Estados', command=selecionar_dados_estado, bg='yellow', fg='black', width=45, takefocus=False, bd=3)
botao_visualizar_estado.grid(row=3, column=2, padx=10, pady=10)

botao_cadastrar_senhas = tk.Button(aba_funcionario, text=' Cadastro login geral', command= criar_janela_cadastro_senhageral, bg='blue', fg='white', bd=3, width=45, takefocus=False)
botao_cadastrar_senhas.grid(row=4, column=1, padx=10, pady=10)

botao_visualizar_pagamento = tk.Button(aba_funcionario, text='Consultar Folha de Pagamento CPF', command=criar_janela_exibicao_sp_pagamento, bg='yellow', fg='black', bd=3, width=45, takefocus=False)
botao_visualizar_pagamento.grid(row=4, column=2, padx=10, pady=10)

botao_update_cliente = tk.Button(aba_funcionario, text='Atualizar Cadastro Cliente', command=update_cliente, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_update_cliente.grid(row=5, column=2, padx=10, pady=10)

botao_visualizar_info_vendedor = tk.Button(aba_funcionario, text='Consultar Informações Vendedor', command=criar_janela_exibicao_info_vendedor, bg='blue', fg='white', bd=3, width=45, takefocus=False)
botao_visualizar_info_vendedor.grid(row=5, column=1, padx=10, pady=10)

botao_update_funcionario = tk.Button(aba_funcionario, text='Atualizar Funcionario', command=update_funcionario, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_update_funcionario.grid(row=6, column=1, padx=10, pady=10)

botao_deletar_funcionario = tk.Button(aba_funcionario, text='Excluir Funcionário', command=delete_funcionario, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_funcionario.grid(row=6, column=2, padx=10, pady=10)

botao_deletar_cargo = tk.Button(aba_funcionario, text='Excluir Cargo', command=delete_cargo, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_cargo.grid(row=7, column=1, padx=10, pady=10)


# ABA COMERCIAL
aba_comercial = ttk.Frame(notebook, takefocus=False)
aba_comercial.grid(row=0, column=2)

notebook.add(aba_comercial, text='Comercial/Vendas')

botao_cadastrar_area_vendedor = tk.Button(aba_comercial, text='Cadastrar área de Atuação Vendedor', command=criar_janela_cadastro_area, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_area_vendedor.grid(row=0, column=0, padx=10, pady=10)

botao_visualizar_area = tk.Button(aba_comercial, text='Visualizar Área de Atuação Vendedor', command=selecionar_dados_area, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botao_visualizar_area.grid(row=0, column=1, padx=10, pady=10)

botao_cadastrar_meta = tk.Button(aba_comercial, text='Cadastrar Meta Vendedor', command=criar_janela_cadastro_meta, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_meta.grid(row=1, column=0, padx=10, pady=10)

botao_visualizar_meta = tk.Button(aba_comercial, text='Visualizar Meta', command=selecionar_dados_meta, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botao_visualizar_meta.grid(row=1, column=1, padx=10, pady=10)

botao_cadastrar_produto = tk.Button(aba_comercial, text='Cadastrar Produto', command=criar_janela_cadastro_produto, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_produto.grid(row=2, column=0, padx=10, pady=10)

botao_visualizar_produto = tk.Button(aba_comercial, text='Visualizar produto', command=selecionar_dados_produto, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botao_visualizar_produto.grid(row=2, column=1, padx=10, pady=10)

botao_cadastrar_pedido = tk.Button(aba_comercial, text='Realizar Pedido', command=criar_cadastro_pedido, bg='blue', fg='white', takefocus=False, width=45, bd=3)
botao_cadastrar_pedido.grid(row=3, column=0, padx=10, pady=10)

botao_visualizar_pedido = tk.Button(aba_comercial, text='Visualizar Pedido', command=selecionar_dados_pedido, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botao_visualizar_pedido.grid(row=3, column=1, padx=10, pady=10)

botao_visualizar_historico_meta = tk.Button(aba_comercial, text='Visualizar Histórico Metas', command=selecionar_dados_historico, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botao_visualizar_historico_meta.grid(row=4, column=1, padx=10, pady=10)

botao_visualizar_fabricacao = tk.Button(aba_comercial, text='Produto em Fabricação', command=selecionar_dados_fabricacao, bg='blue', fg='white', width=45, bd=3, takefocus=False)
botao_visualizar_fabricacao.grid(row=4, column=0, padx=10, pady=10)

botao_visualizar_info_vendedor = tk.Button(aba_comercial, text='Consultar Informações Vendedores', command=criar_janela_exibicao_info_vendedor, bg='blue', fg='white', bd=3, takefocus=False, width=45)
botao_visualizar_info_vendedor.grid(row=5, column=0, padx=10, pady=10)

botao_visualizar_status = tk.Button(aba_comercial, text='Consultar Status Pedido', command=criar_janela_exibicao_status_pedido, bg='yellow', fg='black', bd=3, width=45, takefocus=False)
botao_visualizar_status.grid(row=5, column=1, padx=10, pady=10)

botao_atualizar_area = tk.Button(aba_comercial, text='Atualizar Área Vendedor', command=update_area_vendedor, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_atualizar_area.grid(row=6, column=0, padx=10, pady=10)

botao_atualizar_produto = tk.Button(aba_comercial, text='Atualizar Produto', command=update_produto, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_atualizar_produto.grid(row=6, column=1, padx=10, pady=10)

botao_atualizar_meta = tk.Button(aba_comercial, text='Atualizar Meta', command=update_meta, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_atualizar_meta.grid(row=7, column=0, padx=10, pady=10)

botao_update_cliente = tk.Button(aba_comercial, text='Atualizar Cadastro Cliente', command=update_cliente, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_update_cliente.grid(row=7, column=1, padx=10, pady=10)

botao_deletar_cliente = tk.Button(aba_comercial, text='Excluir Cliente', command=delete_cliente, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_cliente.grid(row=8, column=0, padx=10, pady=10)

botao_deletar_meta = tk.Button(aba_comercial, text='Excluir Meta', command=delete_meta, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_meta.grid(row=8, column=1, padx=10, pady=10)

botao_deletar_produto = tk.Button(aba_comercial, text='Excluir Produto', command=delete_produto, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_produto.grid(row=9, column=0, padx=10, pady=10)

botao_deletar_produto_em_fabricacao = tk.Button(aba_comercial, text='Excluir Fabricação', command=delete_fabricacao, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_produto_em_fabricacao.grid(row=9, column=1, padx=10, pady=10)


# ABA ADMINISTRATIVO

aba_administrativo = ttk.Frame(notebook, takefocus=False)
aba_administrativo.grid(row=0, column=3)

notebook.add(aba_administrativo, text='Administrativo')

botao_cadastro_fornecedor = tk.Button(aba_administrativo, text='Cadastrar Fornecedor', command=criar_janela_cadastro_fornecedor, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastro_fornecedor.grid(row=0, column=1, padx=10, pady=10)

botoao_visualizar_fornecedor = tk.Button(aba_administrativo, text='Visualizar Fornecedor', command=selecionar_dados_fornecedor, bg='yellow', fg='black', takefocus=False, bd=3, width=45)
botoao_visualizar_fornecedor.grid(row=0, column=2, padx=10, pady=10)

botao_gerar_nf = tk.Button(aba_administrativo, text='Gerar Nota Fiscal', command=cadastro_nf, bg='blue', fg='white', bd=3, width=45, takefocus=False)
botao_gerar_nf.grid(row=1, column=1, padx=10, pady=10)

botao_visualizar_notas = tk.Button(aba_administrativo, text='Visualizar Notas', command=selecionar_dados_nf, bg='yellow', fg='black', bd=3, width=45, takefocus=False)
botao_visualizar_notas.grid(row=1, column=2, padx=10, pady=10)

botao_pagamento = tk.Button(aba_administrativo, text='Gerar folha de pagamento', command=cadastro_folha,  bg='blue', fg='white', bd=3, takefocus=False, width=45)
botao_pagamento.grid(row=2, column=1, padx=10, pady=10)

botao_visualizar_folha = tk.Button(aba_administrativo, text='Visualizar folha de pagamento', command=selecionar_dados_folha, bg='yellow', fg='black', bd=3, takefocus=False, width=45)
botao_visualizar_folha.grid(row=2, column=2, padx=10, pady=10)

botao_visualizar_status = tk.Button(aba_administrativo, text='Consultar Status Pedido', command=criar_janela_exibicao_status_pedido, bg='blue', fg='white', bd=3, width=45, takefocus=False)
botao_visualizar_status.grid(row=3, column=1, padx=10, pady=10)

botao_update_cliente2 = tk.Button(aba_administrativo, text='Atualizar Cadastro Cliente', command=update_cliente, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_update_cliente2.grid(row=3, column=2, padx=10, pady=10)

botao_visualizar_fabricacao = tk.Button(aba_administrativo, text='Produto em Fabricação', command=selecionar_dados_fabricacao, bg='blue', fg='white', width=45, bd=3, takefocus=False)
botao_visualizar_fabricacao.grid(row=4, column=1, padx=10, pady=10)

botao_atualizar_fornecedor = tk.Button(aba_administrativo, text='Atualizar Fornecedor', command=update_fornecedor, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_atualizar_fornecedor.grid(row=4, column=2, padx=10, pady=10)

botao_deletar_fornecedor = tk.Button(aba_administrativo, text='Exlcuir Fornecedor', command=delete_fornecedor, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_fornecedor.grid(row=5, column=1, padx=10, pady=10)

botao_deletar_nota = tk.Button(aba_administrativo, text='Exlcuir Nota Fiscal', command=delete_nota, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_nota.grid(row=5, column=2, padx=10, pady=10)


# ABA ESTOQUE/PRODUÇÃO
aba_estoque = ttk.Frame(notebook, takefocus=False)
aba_estoque.grid(row=0, column=4)

notebook.add(aba_estoque, text='Estoque/Produção')

botao_cadastro_estoque = tk.Button(aba_estoque, text='Cadastrar Estoque de Material', command=criar_janela_cadastro_estoque, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastro_estoque.grid(row=0, column=1, padx=10, pady=10)

botao_visualizar_estoque = tk.Button(aba_estoque, text='Visualizar Estoque', command=selecionar_dados_estoque, bg='yellow', fg='black', width=45, takefocus=False, bd=3)
botao_visualizar_estoque.grid(row=0, column=2, padx=10, pady=10)

botao_cadastrar_material_consumo = tk.Button(aba_estoque, text='Saída de Material', command=cadastro_saida_material, bg='blue', fg='white', width=45, takefocus=False, bd=3)
botao_cadastrar_material_consumo.grid(row=1, column=1, padx=10, pady=10)

botao_visualizar_consumo = tk.Button(aba_estoque, text='Visualizar saídas de Material', command=selecionar_dados_material_consumo, bg='yellow', fg='black', width=45, takefocus=False, bd=3)
botao_visualizar_consumo.grid(row=1, column=2, padx=10, pady=10)

botao_visualizar_fabricacao = tk.Button(aba_estoque, text='Produto em Fabricação', command=selecionar_dados_fabricacao, bg='blue', fg='white', width=45, bd=3, takefocus=False)
botao_visualizar_fabricacao.grid(row=2, column=1, padx=10, pady=10)

botao_visualizar_materiais = tk.Button(aba_estoque, text='Consultar Material por Pedido', command=criar_janela_exibicao_material_pedido, bg='yellow', fg='black', bd=3, width=45, takefocus=False)
botao_visualizar_materiais.grid(row=2, column=2, padx=10, pady=10)

botao_visualizar_status2 = tk.Button(aba_estoque, text='Consultar Status Pedido', command=criar_janela_exibicao_status_pedido, bg='blue', fg='white', bd=3, width=45, takefocus=False)
botao_visualizar_status2.grid(row=3, column=1, padx=10, pady=10)

botao_atualizar_estoque = tk.Button(aba_estoque, text='Atualizar Estoque de Material', command=update_estoque, bg='green', fg='white', bd=3, width=45, takefocus=False)
botao_atualizar_estoque.grid(row=3, column=2, padx=10, pady=10)

botao_deletar_saida = tk.Button(aba_estoque, text='Deletar saída', command=delete_saida, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_saida.grid(row=4, column=1, padx=10, pady=10)

botao_deletar_producao = tk.Button(aba_estoque, text='Deletar Produção', command=delete_produto_fabricacao, bg='red', fg='white', bd=3, width=45, takefocus=False)
botao_deletar_producao.grid(row=4, column=2, padx=10, pady=10)

# Definir qual aba será aberta quando iniciar o programa
notebook.select(aba_estoque)


# ############################### SENSOR PARA CLIQUE EM ABA ############################################################
cont_janela_login = 0


def exibir_mensagem_aba(event):
    global cont_janela_login

    # Obtém o índice da aba atual
    current_tab_index = notebook.index(notebook.select())

    # Verifica se a aba atual é uma das abas desejadas
    if current_tab_index in [0, 1, 2, 3]:  # Substitua 0, 1, 2 pelos índices das abas desejadas
        if cont_janela_login == 0:
            cont_janela_login += 1
            criar_janela_login()


# Adicione estas linhas para vincular a função aos eventos de clique nas abas desejadas
notebook.bind("<ButtonRelease-1>", exibir_mensagem_aba)
notebook.bind("<ButtonRelease-2>", exibir_mensagem_aba)
notebook.bind("<ButtonRelease-3>", exibir_mensagem_aba)
notebook.bind("<ButtonRelease-4>", exibir_mensagem_aba)

# ####################################### LIMPAR TELA ##################################################################


def limpar_tela():
    entry_nome.delete(0, tk.END)
    entry_cpf.delete(0, tk.END)
    entry_cnpj.delete(0, tk.END)
    entry_email.delete(0, tk.END)
    entry_rua.delete(0, tk.END)
    entry_numero.delete(0, tk.END)
    entry_bairro.delete(0, tk.END)
    entry_cidade.delete(0, tk.END)
    entry_estado.delete(0, tk.END)
    entry_ddd.delete(0, tk.END)
    entry_telefone.delete(0, tk.END)


def limpar_tela_funcionario():
    entry_data_admissao.delete(0, tk.END)
    entry_nome_func.delete(0, tk.END)
    entry_cpf_func.delete(0, tk.END)
    entry_pis_func.delete(0, tk.END)
    entry_rua_func.delete(0, tk.END)
    entry_numero_func.delete(0, tk.END)
    entry_bairro_func.delete(0, tk.END)
    entry_cidade_func.delete(0, tk.END)
    entry_estado_func.delete(0, tk.END)
    entry_ddd_func.delete(0, tk.END)
    entry_telefone_func.delete(0, tk.END)
    entry_sexo_func.delete(0, tk.END)
    entry_idcargo_func.delete(0, tk.END)
    entry_status_func.delete(0, tk.END)
    entry_salario_func.delete(0, tk.END)


def limpar_tela_cargo():
    entry_nome_cargo.delete(0, tk.END)


def limpar_tela_estado():
    entry_estado1.delete(0, tk.END)


def limpar_tela_area():
    entry_idfuncionario.delete(0, tk.END)
    entry_idestado.delete(0, tk.END)


def limpar_tela_meta():
    entry_idfuncionariom.delete(0, tk.END)
    entry_valor_meta.delete(0, tk.END)
    entry_data_final_meta.delete(0, tk.END)


def limpar_tela_produto():
    entry_nomep.delete(0, tk.END)
    entry_tipo.delete(0, tk.END)
    entry_cor.delete(0, tk.END)
    entry_turbo.delete(0, tk.END)
    entry_porta.delete(0, tk.END)
    entry_preco.delete(0, tk.END)


def limpar_tela_fornecedor():
    entry_nomef.delete(0, tk.END)
    entry_tipomaterial.delete(0, tk.END)
    entry_cnpjf.delete(0, tk.END)
    entry_dddf.delete(0, tk.END)
    entry_telefonef.delete(0, tk.END)
    entry_emailf.delete(0, tk.END)
    entry_ruaf.delete(0, tk.END)
    entry_nf.delete(0, tk.END)
    entry_bairrof.delete(0, tk.END)
    entry_cidadef.delete(0, tk.END)
    entry_estadof.delete(0, tk.END)
    entry_situacaof.delete(0, tk.END)


def limpar_tela_cadastro_estoque():
     entry_id_fornecedor.delete(0, tk.END)
     entry_lote.delete(0, tk.END)
     entry_nome_produto.delete(0, tk.END)
     entry_tipo_material.delete(0, tk.END)
     entry_cor_material.delete(0, tk.END)
     entry_tamanho.delete(0, tk.END)
     entry_quantidade.delete(0, tk.END)
     entry_data_validade.delete(0, tk.END)


def limpar_tela_cadastro_pedido():
    entry_id_cliente.delete(0, tk.END)
    entry_id_produto.delete(0, tk.END)
    entry_id_vendedor.delete(0, tk.END)
    entry_quantidade_p.delete(0, tk.END)
    entry_data_entrega_p.delete(0, tk.END)
    entry_modo_pagamento.delete(0, tk.END)
    entry_parcelas.delete(0, tk.END)


def limpar_tela_saida_material():
    entry_id_pedido.delete(0, tk.END)
    entry_id_estoque.delete(0, tk.END)
    entry_quantidade_consumo.delete(0, tk.END)


def limpar_tela_pagamento_folha():
    entry_id_funcionario.delete(0, tk.END)
    entry_calculo.delete(0, tk.END)


def limpar_tela_update_cliente():
    entry_nomeup.delete(0, tk.END)
    entry_nascimentoup.delete(0, tk.END)
    entry_cnpjup.delete(0, tk.END)
    entry_emailup.delete(0, tk.END)
    entry_ruaup.delete(0, tk.END)
    entry_numeroup.delete(0, tk.END)
    entry_bairroup.delete(0, tk.END)
    entry_cidadeup.delete(0, tk.END)
    entry_estadoup.delete(0, tk.END)
    entry_dddup.delete(0, tk.END)
    entry_telefoneup.delete(0, tk.END)
    entry_cpfup.delete(0, tk.END)


def limpar_tela_update_funcionario():
    entry_nomeup.delete(0, tk.END)
    entry_pisup.delete(0, tk.END)
    entry_ruaup.delete(0, tk.END)
    entry_nup.delete(0, tk.END)
    entry_bairroup.delete(0, tk.END)
    entry_cidadeup.delete(0, tk.END)
    entry_estadoup.delete(0, tk.END)
    entry_dddup.delete(0, tk.END)
    entry_telefoneup.delete(0, tk.END)
    entry_idcargo.delete(0, tk.END)
    entry_status.delete(0, tk.END)
    entry_idsalario.delete(0, tk.END)
    entry_cpfup1.delete(0, tk.END)


def limpar_tela_update_area():
    entry_id_estado.delete(0, tk.END)
    entry_idvendedor.delete(0, tk.END)


def limpar_tela_update_produto():
    entry_nomepro.delete(0, tk.END)
    entry_desc_pro.delete(0, tk.END)
    entry_cor_pro.delete(0, tk.END)
    entry_turbo_pro.delete(0, tk.END)
    entry_portas_pro.delete(0, tk.END)
    entry_preco_pro.delete(0, tk.END)
    entry_idproduto.delete(0, tk.END)


def limpar_tela_update_meta():
    entry_metaa.delete(0, tk.END)
    entry_datam.delete(0, tk.END)
    entry_idmeta.delete(0, tk.END)


def limpar_tela_tela_fornecedor():
    entry_nomefor.delete(0, tk.END)
    entry_tipofor.delete(0, tk.END)
    entry_cnpjfor.delete(0, tk.END)
    entry_dddforn.delete(0, tk.END)
    entry_telefoneforn.delete(0, tk.END)
    entry_emailforn.delete(0, tk.END)
    entry_nforn.delete(0, tk.END)
    entry_ruaforn.delete(0, tk.END)
    entry_bairroforn.delete(0, tk.END)
    entry_cidadeforn.delete(0, tk.END)
    entry_estadoforn.delete(0, tk.END)
    entry_situacaoforn.delete(0, tk.END)
    entry_idfornecedorn.delete(0, tk.END)


def limpar_tela_update_estoque():
    entry_id_fornecedor_estoque.delete(0, tk.END)
    entry_loteest.delete(0, tk.END)
    entry_nomeest.delete(0, tk.END)
    entry_tipoest.delete(0, tk.END)
    entry_corest.delete(0, tk.END)
    entry_tamanhoest.delete(0, tk.END)
    entry_qtdest.delete(0, tk.END)
    entry_dataest.delete(0, tk.END)
    entry_idestoque.delete(0, tk.END)


def limpar_tela_cadastro_login():
    entry_login.delete(0, tk.END)
    entry_senha.delete(0, tk.END)


janela.mainloop()



# colocar data de nascimento do funcionario tanto aqui, quanto no banco

