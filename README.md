# universidade
Criando tabela alunos

´´´
create table Alunos 
(
raAluno int primary key auto_increment,
nomeAluno varchar(45),
sobrenomeAluno varchar(45),
idCurso int references Cursos(idCurso),
emailAluno varchar(100)
);
´´´

Criando tabela professores

create table Professores
(
idProfessor int primary key auto_increment,
nomeProfessor varchar(100)
);


Criando tabela cursos

create table Cursos
(
idCurso int primary key auto_increment,
nomeCurso varchar(100)
);


Criando tabela professores e cursos

create table ProfessoresCursos
(
idProfessor int references Professores(idProfessor),
idCurso int references Cursos(idCurso)
);


Criando stored procedure para inserir curso

DELIMITER //

CREATE PROCEDURE InserirCurso (
    IN nomeCursoParam VARCHAR(100)
)
BEGIN
    -- Inserir o curso na tabela Cursos
    INSERT INTO Cursos (nomeCurso)
    VALUES (nomeCursoParam);
END //

DELIMITER ;

CALL InserirCurso('Matemática');
CALL InserirCurso('História');
CALL InserirCurso('Ciências');
CALL InserirCurso('Inglês');
CALL InserirCurso('Física');


Criando stored procedure para inserir aluno

DELIMITER //

CREATE PROCEDURE InserirAluno (
    IN nomeAlunoParam VARCHAR(45),
    IN sobrenomeAlunoParam VARCHAR(45),
    IN idCursoParam INT
)
BEGIN
    DECLARE emailAlunoParam VARCHAR(100);

    SET emailAlunoParam = CONCAT(nomeAlunoParam, '.', sobrenomeAlunoParam,  '@facens.br');

    INSERT INTO Alunos (nomeAluno, sobrenomeAluno, emailAluno, idCurso)
    VALUES (nomeAlunoParam, sobrenomeAlunoParam, emailAlunoParam, idCursoParam);
END //

DELIMITER ;


Criando stored procedure para inserir professor

DELIMITER //

CREATE PROCEDURE InserirProfessor (
    IN nomeProfessorParam VARCHAR(100)
)
BEGIN
    -- Inserir o professor na tabela Professores
    INSERT INTO Professores (nomeProfessor)
    VALUES (nomeProfessorParam);
END //

DELIMITER ;

CALL InserirAluno('João', 'Silva', 1);
CALL InserirAluno('Maria', 'Santos', 2);
CALL InserirAluno('Pedro', 'Ferreira', 2);
CALL InserirAluno('Ana', 'Oliveira', 3);
CALL InserirAluno('Rafael', 'Lima', 5);
CALL InserirAluno('Camila', 'Mendes', 2);
CALL InserirAluno('Gustavo', 'Pereira', 3);
CALL InserirAluno('Carla', 'Rodrigues', 4);
CALL InserirAluno('Lucas', 'Fernandes', 2);
CALL InserirAluno('Isabela', 'Martins', 1);

CALL InserirProfessor('Prof. Carlos');
CALL InserirProfessor('Prof. Ana');
CALL InserirProfessor('Prof. Miguel');
CALL InserirProfessor('Prof. Sofia');
CALL InserirProfessor('Prof. José');
CALL InserirProfessor('Prof. Clara');
CALL InserirProfessor('Prof. André');
CALL InserirProfessor('Prof. Laura');
CALL InserirProfessor('Prof. Guilherme');
CALL InserirProfessor('Prof. Beatriz');


Criando stored procedure para associar professor e curso

DELIMITER //

CREATE PROCEDURE AssociarProfessorCurso (
    IN idProfessorParam INT,
    IN idCursoParam INT
)
BEGIN
    INSERT INTO ProfessoresCursos (idProfessor, idCurso)
    VALUES (idProfessorParam, idCursoParam);
END //

DELIMITER ;

CALL AssociarProfessorCurso(1, 1); 
CALL AssociarProfessorCurso(2, 2); 
CALL AssociarProfessorCurso(3, 3);  
CALL AssociarProfessorCurso(4, 4);  
CALL AssociarProfessorCurso(5, 5);


Criando stored procedure para consultar os dados

DELIMITER //

CREATE PROCEDURE ConsultarDadosCursoProfessorAlunos ()
BEGIN
    SELECT Cursos.nomeCurso AS Curso,
           Professores.nomeProfessor AS Professor,
           GROUP_CONCAT(CONCAT(Alunos.nomeAluno, ' ', Alunos.sobrenomeAluno) SEPARATOR ', ') AS Alunos
    FROM Cursos
    JOIN ProfessoresCursos ON Cursos.idCurso = ProfessoresCursos.idCurso
    JOIN Professores ON ProfessoresCursos.idProfessor = Professores.idProfessor
    LEFT JOIN Alunos ON Cursos.idCurso = Alunos.idCurso
    GROUP BY Cursos.nomeCurso;
END //

DELIMITER ;

