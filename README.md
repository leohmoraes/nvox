# Docker PHP5.6 Apache

### Building 

    $ docker build -t php-server github.com/leohmoraes/nvox.git

### Building with git clone 

    $ git clone github.com/leohmoraes/nvox.git PATH
    # docker build -t nomeContainer PATH
    $ docker build -t php56 PATH


## Run
    # Roda na porta 80 (da maquina principal/host) o container php56 criado acima, e o diretorio home é onde foi executado.
    $ docker run -d -p 80:80 -v $PWD:/var/www php56

## Run with PHPMySql and MySql

MySql

    $ docker run \
        --name mysql57 \
        -v $PWD/mysql:/var/lib/mysql \
        -p 3306:3306 \
        -e MYSQL_ROOT_PASSWORD=123 \
        -d mysql:5.7

PHP and Apache

    $ docker run \
        --name php56 \
        -p 80:80 \
        -v $PWD:/var/www \
        --link mysql57:db \
        -d php56

PHPMyAdmin

    $ docker run \
        --name myadmin \
        -d --link mysql57:db \
        -p 8000:80 \
        -e PMA_HOST="172.17.0.2" \
        phpmyadmin/phpmyadmin

## Extras

#### composer install

    $ composer install --ignore-platform-reqs --no-scripts


### Teste de Conexao
````
<?php

echo 'Versão Atual do PHP: ' . phpversion();
$servername = "mysql57";
$username = "root";
$password = "root";
$conn = new mysqli($servername, $username, $password);

// Check connection
if ($conn->connect_error) {
    die("<br />Connection failed: " . $conn->connect_error);
}
echo "<br /> Connected successfully";
?>
````
