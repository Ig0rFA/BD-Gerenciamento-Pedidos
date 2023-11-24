# BD-Gerenciamento-Pedidos
Nesta atividade iremos criar um sistema de gerenciamento de pedidos em um banco de dados utilizando stored procedures, triggers, views e JOINs no MySQL Workbench.

## Etapa 1: Criação de Tabelas e Inserção de Dados
```
create table clientes(
id int  primary key auto_increment not null,
nome varchar(80),
email varchar(100),
telefone varchar(800),
endereco varchar(100)
);
	
insert into clientes (nome, email, telefone, endereco) values ('Igor','igor@gmail.com', '111222','rua sao paulo'),
insert into clientes (nome, email, telefone, endereco) values ('Joao','joao@gmail.com','333444','rua oliveiras filho'),
insert into clientes (nome, email, telefone, endereco) values ('Eduardo', 'eduardo@gmail.com', '555666', 'rua aparecido leme'),

create table pedidos(
id int primary key auto_increment not null,
data_pedido date,
valor decimal(10,2),
cliente_id int,
foreign key (cliente_id) references clientes(id)
);

insert into pedidos  (data_pedido, valor, cliente_id) values('2022-01-06', 100.50, 1),
insert into pedidos  (data_pedido, valor, cliente_id) values('2023-02-07',200.50, 2),
insert into pedidos  (data_pedido, valor, cliente_id) values('2024-03-08', 300.50, 3),
```
## Etapa 2: Criação de Stored Procedure
```
DELIMITER $$

create procedure InsePedido(
in cliente_id int,
in data_pedido date,
in valor decimal(10,2)
)
begin
	insert into pedidos(cliente_id, data_pedido, valor) values (cliente_id, data_pedido, valor);
end$$

DELIMITER ;

call InsePedido(5,'2024-01-01',150.00);

select * from pedidos;
```
## Etapa 3: Trigger

alter table clientes add TotalPedidos decimal(10, 2);
```
DELIMITER $$
create trigger AtualizaTotalPedidos 
after insert on pedidos
for each row
begin
update clientes
	set TotalPedidos = TotalPedidos + new.valor
	where id = new.cliente_id;
end$$
DELIMITER ;


	INSERT INTO Pedidos (cliente_id, data_pedido, valor)
	VALUES (2, '2021-05-08', 80.00);

select * from clientes;
```

## Etapa 4: View
 ```
 create view PedidosClientes as
    select 
    clientes.nome, 
    pedidos.valor, 
    pedidos.data_pedido, 
    pedidos.cliente_id
    from clientes
    inner join pedidos
    on clientes.id = pedidos.cliente_id;
```

## Etapa 5: Consulta com JOIN
```
	select * from clientes inner join PedidosClientes;
  ```
