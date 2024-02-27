# banco-dados-revista
Banco de dados com Rust

Criar um banco de dados de revistas no PostgreSQL com Rust
Requisitos:

Rust instalado
Cargo instalado
PostgreSQL instalado e configurado
Conhecimento básico de Rust e PostgreSQL
Passos:

# 1. Criar um novo projeto Rust:
cargo new revista
# 2. Adicionar as dependências necessárias ao arquivo Cargo.toml:
[dependencies]
postgres = { version = "0.18.2" }
serde = { version = "1.0.136", features = ["derive"] }
# 3. Criar o modelo de dados:
Rust
#[derive(Debug, Serialize, Deserialize)]
struct Revista {
    id: i32,
    titulo: String,
    autor: String,
    ano: i32,
}
# 4. Criar a função para conectar ao banco de dados:
Rust
fn conectar() -> Result<PgConnection, Error> {
    let url = "postgres://postgres:postgres@localhost:5432/revista";
    PgConnection::connect(url)
}
# 5. Criar a função para inserir uma revista no banco de dados:
Rust
fn inserir_revista(revista: &Revista) -> Result<(), Error> {
    let mut conn = conectar()?;
    let query = "INSERT INTO revistas (titulo, autor, ano) VALUES ($1, $2, $3)";
    conn.execute(query, &[&revista.titulo, &revista.autor, &revista.ano])?;
    Ok(())
}
# 6. Criar a função para consultar as revistas no banco de dados:
Rust
fn consultar_revistas() -> Result<Vec<Revista>, Error> {
    let mut conn = conectar()?;
    let query = "SELECT * FROM revistas";
    let rows = conn.query(query)?;
    let mut revistas = Vec::new();
    for row in rows {
        let revista = Revista {
            id: row.get("id"),
            titulo: row.get("titulo"),
            autor: row.get("autor"),
            ano: row.get("ano"),
        };
        revistas.push(revista);
    }
    Ok(revistas)
}
# 7. Criar a função principal:
Rust
fn main() {
    let revista = Revista {
        id: 1,
        titulo: "Minha revista".to_string(),
        autor: "Eu".to_string(),
        ano: 2024,
    };

    inserir_revista(&revista).unwrap();

    let revistas = consultar_revistas().unwrap();

    for revista in revistas {
        println!("{:?}", revista);
    }
}
# 8. Executar o programa:
cargo run
Observações:

Este é um exemplo simples. Você pode adaptar o código para atender às suas necessidades específicas.
Você pode usar outras bibliotecas Rust para trabalhar com PostgreSQL, como diesel.
Você pode encontrar mais informações sobre o PostgreSQL em https://www.postgresql.org/.
Você pode encontrar mais informações sobre Rust em https://www.rust-lang.org/.
Recursos:

https://www.postgresql.org/
https://www.rust-lang.org/
Livro Rust: https://doc.rust-lang.org/book/
Documentação do PostgreSQL: https://www.postgresql.org/docs/
