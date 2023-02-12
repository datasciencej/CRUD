import sqlite3

# Conecta ao banco de dados (cria um se não existir)
conn = sqlite3.connect('database.db')
cursor = conn.cursor()

# Cria a tabela "usuarios"
cursor.execute("""
CREATE TABLE IF NOT EXISTS usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT,
    idade INTEGER
)
""")
conn.commit()

# Insere dados na tabela
def insert_user(nome, idade):
    cursor.execute("""
    INSERT INTO usuarios (nome, idade)
    VALUES (?,?)
    """, (nome, idade))
    conn.commit()
    print(f"Usuário {nome} adicionado com sucesso.")

# Seleciona todos os dados da tabela
def select_all_users():
    cursor.execute("""
    SELECT * FROM usuarios
    """)
    return cursor.fetchall()

# Seleciona um usuário pelo ID
def select_user(user_id):
    cursor.execute("""
    SELECT * FROM usuarios
    WHERE id = ?
    """, (user_id,))
    return cursor.fetchone()

# Atualiza um usuário pelo ID
def update_user(user_id, nome, idade):
    cursor.execute("""
    UPDATE usuarios
    SET nome = ?, idade = ?
    WHERE id = ?
    """, (nome, idade, user_id))
    conn.commit()
    print(f"Usuário com ID {user_id} atualizado com sucesso.")

# Deleta um usuário pelo ID
def delete_user(user_id):
    cursor.execute("""
    DELETE FROM usuarios
    WHERE id = ?
    """, (user_id,))
    conn.commit()
    print(f"Usuário com ID {user_id} deletado com sucesso.")

# Insere alguns dados de exemplo
insert_user("João Silva", 30)
insert_user("Maria Souza", 25)

# Seleciona e exibe todos os usuários
print("Todos os usuários:")
for user in select_all_users():
    print(user)

# Seleciona e exibe o usuário com ID 1
print("Usuário com ID 1:")
print(select_user(1))

# Atualiza o usuário com ID 1
update_user(1, "João da Silva", 31)

# Seleciona e exibe novamente todos os usuários
print("Todos os usuários:")
for user in select_all_users():
    print(user)