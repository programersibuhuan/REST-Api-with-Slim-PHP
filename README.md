<div align="center">
	<h1> REST Api with Slim PHP </h1>
</div>

<div align="center">
	<a href="#changelog">
		<img src="https://img.shields.io/badge/stability-stable-green.svg" alt="Status">
	</a>
	<a href="#changelog">
		<img src="https://img.shields.io/badge/release-v1.0.0.3-blue.svg" alt="Version">
	</a>
	<a href="#changelog">
		<img src="https://img.shields.io/badge/update-july-yellowgreen.svg" alt="Update">
	</a>
	<a href="#license">
		<img src="https://img.shields.io/badge/license-MIT%20License-green.svg" alt="License">
	</a>
</div>

This API works with the same concept of social network of [Fav Quote](http://fav-quote.byethost17.com).

This is a simple REST Web Service which allow:

  * Post short text messages of no more than 120 characters
  * Bring a list with the latest published messages
  * Search for messages by your text
  * Delete a specific message by its id

<a name="started"></a>
## :traffic_light: Getting Started

This page will help you get started with this API.

<a name="requirements"></a>
### Requirements

  * PHP 5.6
  * MySQL or MariaDB
  * Apache Server

<a name="installation"></a>
### Installation

#### Copy this project

  1. Clone or Download this repository
  2. Unzip the archive if needed
  3. Copy the folder in the htdocs dir
  4. Start a Text Editor (Atom, Sublime, Visual Studio Code, Vim, etc)
  5. Add the project folder to the editor

#### Install the project

  1. Go to htdocs dir

  * Windows

```bash
$ cd /d C:\xampp\htdocs
```

  * Linux

```bash
$ cd /opt/lampp/htdocs
```

  * MAC

```bash
$ cd applications/mamp/htdocs
```

  2. Go to the project folder

```bash
$ cd REST-Api-with-Slim-PHP
```

  3. Install with composer

```bash
$ composer install
```

    Or

```bash
$ php composer.phar install  
```

#### Create a database

  Import the [NETWORK SCHEMA DDL.sql](https://github.com/ManuelGil/REST-Api-with-Slim-PHP/blob/master/install/NETWORK%20SCHEMA%20DDL.sql)
 file.

  Import the [NETWORK SCHEMA DML.sql](https://github.com/ManuelGil/REST-Api-with-Slim-PHP/blob/master/install/NETWORK%20SCHEMA%20DML.sql)
 file.

  Or run the following SQL script

```SQL
SET @OLD_AUTOCOMMIT=@@AUTOCOMMIT, AUTOCOMMIT=0;
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL,ALLOW_INVALID_DATES';

START TRANSACTION;

-- -----------------------------------------------------
-- Schema NETWORK
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `NETWORK` DEFAULT CHARACTER SET utf8 ;
USE `NETWORK` ;

-- -----------------------------------------------------
-- Table `NETWORK`.`COUNTRIES`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `NETWORK`.`COUNTRIES` (
  `ID_COUNTRY` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `ISO` VARCHAR(2) NOT NULL,
  `COUNTRY` VARCHAR(80) NOT NULL,
  PRIMARY KEY (`ID_COUNTRY`))
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Dumping data for table `NETWORK`.`COUNTRIES`
-- -----------------------------------------------------
INSERT INTO `NETWORK`.`COUNTRIES` (`ID_COUNTRY`, `ISO`, `COUNTRY`) VALUES
(1, 'AF', 'Afghanistan');

-- -----------------------------------------------------
-- Table `NETWORK`.`USERS`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `NETWORK`.`USERS` (
  `ID_USER` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `GUID` VARCHAR(20) NOT NULL,
  `USERNAME` VARCHAR(20) NOT NULL,
  `PASSWORD` VARCHAR(255) NOT NULL,
  `CREATED_AT` DATE NOT NULL,
  `ID_COUNTRY` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`ID_USER`),
  UNIQUE INDEX `ID_USER_UNIQUE` (`ID_USER` ASC),
  UNIQUE INDEX `USER_UNIQUE` (`USERNAME` ASC),
  UNIQUE INDEX `GUID_UNIQUE` (`GUID` ASC),
  INDEX `fk_USERS_COUNTRIES1_idx` (`ID_COUNTRY` ASC),
  CONSTRAINT `fk_USERS_COUNTRIES1`
    FOREIGN KEY (`ID_COUNTRY`)
    REFERENCES `NETWORK`.`COUNTRIES` (`ID_COUNTRY`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Dumping data for table `NETWORK`.`USERS`
-- -----------------------------------------------------
INSERT INTO `NETWORK`.`USERS` (`ID_USER`, `GUID`, `USERNAME`, `PASSWORD`, `CREATED_AT`, `ID_COUNTRY`) VALUES
(0, '', 'ManuelGil', '', '2018-01-01', 1);

-- -----------------------------------------------------
-- Table `NETWORK`.`QUOTES`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `NETWORK`.`QUOTES` (
  `ID_QUOTE` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `QUOTE` VARCHAR(120) NOT NULL,
  `POST_DATE` DATE NOT NULL,
  `POST_TIME` TIME NOT NULL,
  `LIKES` INT UNSIGNED NOT NULL DEFAULT 0,
  `ID_USER` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`ID_QUOTE`),
  UNIQUE INDEX `ID_QUOTE_UNIQUE` (`ID_QUOTE` ASC),
  INDEX `fk_QUOTES_USERS_idx` (`ID_USER` ASC),
  CONSTRAINT `fk_QUOTES_USERS`
    FOREIGN KEY (`ID_USER`)
    REFERENCES `NETWORK`.`USERS` (`ID_USER`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

-- -----------------------------------------------------
-- Dumping data for table `NETWORK`.`QUOTES`
-- -----------------------------------------------------
INSERT INTO `NETWORK`.`QUOTES` (`ID_QUOTE`, `QUOTE`, `POST_DATE`, `POST_TIME`, `LIKES`, `ID_USER`) VALUES
(0, 'Fav Quote is a Micro Social Network with PHP, MySQL, Bootstrap 3 and Vue.JS 2. It don\'t use classes or a php framework.', '2018-01-01', '00:00:00', 1, 0);

-- -----------------------------------------------------
-- Table `NETWORK`.`LIKES`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `NETWORK`.`LIKES` (
  `ID_USER` INT UNSIGNED NOT NULL,
  `ID_QUOTE` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`ID_USER`, `ID_QUOTE`),
  INDEX `fk_LIKES_QUOTES1_idx` (`ID_QUOTE` ASC),
  CONSTRAINT `fk_LIKES_USERS1`
    FOREIGN KEY (`ID_USER`)
    REFERENCES `NETWORK`.`USERS` (`ID_USER`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_LIKES_QUOTES1`
    FOREIGN KEY (`ID_QUOTE`)
    REFERENCES `NETWORK`.`QUOTES` (`ID_QUOTE`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

COMMIT;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```

#### Configure the project

  Copy the [`.env.example`](https://github.com/ManuelGil/REST-Api-with-Slim-PHP/blob/master/.env.example)
 file and call it `.env`.

  Change the database configuration in the new file.

<a name="deployment"></a>
## :package: Deployment

<div align="center">
	<h3> Database Schema </h3>
	<a href="#installation">
		<img src="https://raw.githubusercontent.com/ManuelGil/Simple-Social-Network/master/docs/images/schema.png" alt="schema">
	</a>
</div>

<a name="built"></a>
## :wrench: Built With

  * XAMPP ([XAMPP for Windows 5.6.32](https://www.apachefriends.org/download.html))
  * ATOM ([ATOM](https://atom.io/))
  * COMPOSER ([COMPOSER](https://getcomposer.org/))
  * RestEasy Extension for Chrome ([RestEasy](https://chrome.google.com/webstore/detail/resteasy/nojelkgnnpdmhpankkiikipkmhgafoch))

<a name="test"></a>
## :100: Running the tests

Use RestEasy or Postman app for testing.

For authentication you can generate a new JSON Web Token with the url login.

Put the token on an HTTP header called Authorization. e.g.:

  * Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJoZWFkZXIiOnsiaWQiOiIwIiwidXNlciI6Ik1hbnVlbEdpbCJ9LCJwYXlsb2FkIjp7ImlhdCI6IjIwMTktMDEtMDEgMDA6MDA6MDAiLCJleHAiOiIyMDIwLTAxLTAxIDAwOjAwOjAwIn19.p_PtmXsDe3l_osPEzb4edkf-M2SCdUVBQBKs8ZAbpn8

<div align="center">
	<img src="https://mfgil.files.wordpress.com/2017/12/header.png" alt="Header">
</div>

Put the parameters on a Query Parameter.

<div align="center">
	<img src="https://mfgil.files.wordpress.com/2017/12/test.png" alt="Test">
</div>

Checks if the `iat` (issued at) and `exp` (expiration time) are correct in [https://jwt.io/](https://jwt.io/).

### Routes

  * `/ping` - get method - This method is used for testing the api. e.g.: `/public/webresources/mobile_app/ping`

  * `/login/{user}/{password}` - get method - This method gets a user into the database. e.g.: `/public/webresources/mobile_app/login/testUser/testPwd`

  * `/register/{user}/{password}/{country}` - get method - This method sets a user into the database. e.g.: `/public/webresources/mobile_app/register/testUser/testPwd/1`

  * `/verify` - get method - This method checks the token. e.g.: `/public/webresources/mobile_app/verify`

  * `/post` - post method - This method publish short text messages of no more than 120 characters. e.g.: `/public/webresources/mobile_app/post`

  * `/list` - get method - This method list the latest published messages. e.g.: `/public/webresources/mobile_app/list`

  * `/likes/{id}` - get method - This method list the users for likes. e.g.: `/public/webresources/mobile_app/likes/1`

  * `/search/{quote}` - get method - This method searches for messages by your text. e.g.: `/public/webresources/mobile_app/search/Quote`

  * `/delete` - delete method - This method deletes a specific message by its id. e.g.: `/public/webresources/mobile_app/delete`

<a name="changelog"></a>
## :information_source: Changelog

**1.0.0.3** (07/07/2018)

  * <table border="0" cellpadding="4">
		<tr>
			<td>
				<strong>Language:</strong>
			</td>
			<td>
				PHP
			</td>
		</tr>
		<tr>
			<td><strong>
				Requirements:
			</strong></td>
			<td>
				<ul>
					<li>
						PHP 5.6
					</li>
					<li>
						MySQL or MariaDB 
					</li>
					<li>
						Apache Server
					</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>
				<strong>Changes:</strong>
			</td>
			<td>
				<ul>
					<li>
						DotEnv integration
					</li>
				</ul>
			</td>
		</tr>
	</table>

**1.0.0.2** (03/29/2018)

  * <table border="0" cellpadding="4">
		<tr>
			<td>
				<strong>Language:</strong>
			</td>
			<td>
				PHP
			</td>
		</tr>
		<tr>
			<td><strong>
				Requirements:
			</strong></td>
			<td>
				<ul>
					<li>
						PHP 5.6
					</li>
					<li>
						MySQL or MariaDB 
					</li>
					<li>
						Apache Server
					</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>
				<strong>Changes:</strong>
			</td>
			<td>
				<ul>
					<li>
						Add a new table in database to save likes
					</li>
					<li>
						Add 3 methods (ping, register, likes)
					</li>
					<li>
						Add logger with Monolog
					</li>
					<li>
						Add JSON file for installation with composer
					</li>
				</ul>
			</td>
		</tr>
	</table>

**1.0.0.1** (12/07/2017)

  * <table border="0" cellpadding="4">
		<tr>
			<td>
				<strong>Language:</strong>
			</td>
			<td>
				PHP
			</td>
		</tr>
		<tr>
			<td><strong>
				Requirements:
			</strong></td>
			<td>
				<ul>
					<li>
						PHP 5.6
					</li>
					<li>
						MySQL or MariaDB 
					</li>
					<li>
						Apache Server
					</li>
				</ul>
			</td>
		</tr>
		<tr>
			<td>
				<strong>Changes:</strong>
			</td>
			<td>
				<ul>
					<li>
						Add Authentication with <a href="https://github.com/tuupola/slim-jwt-auth">PSR-7 JWT Authentication Middleware</a>
					</li>
				</ul>
			</td>
		</tr>
	</table>

<a name="Donate"></a>
## :gift: Donate!

If you want to help me to continue this project, you might donate via PayPal.

<a href="https://paypal.me/ManuelFGil"><img src="https://www.paypalobjects.com/webstatic/en_US/i/btn/png/btn_donate_92x26.png" alt="Donate via PayPal"></a>

<a name="authors"></a>
## :eyeglasses: Authors

  * **Manuel Gil** - *Initial work* - [ManuelGil](https://github.com/ManuelGil) 

See also the list of [contributors](https://github.com/ManuelGil/REST-Api-with-Slim-PHP/contributors)
 who participated in this project.

<a name="license"></a>
## :memo: License

This API is licensed under the MIT License - see the
 [MIT License](https://opensource.org/licenses/MIT) for details.
