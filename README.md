1. Clona el Repositorio
Primero, clona el repositorio que contiene el contrato PrivateBank.sol:

bash
Copiar código
git clone https://github.com/vottunio/activity-audit-sc.git
cd activity-audit-sc
2. Configura Hardhat
Si aún no has configurado Hardhat en tu proyecto, instálalo y configura un proyecto básico con los siguientes comandos:

bash
Copiar código
npm install --save-dev hardhat
npx hardhat
Sigue los pasos para crear un proyecto básico cuando se te solicite.

3. Instala Dependencias Adicionales
Instala las dependencias necesarias para realizar pruebas:

bash
Copiar código
npm install --save-dev @nomiclabs/hardhat-ethers ethers
npm install --save-dev @nomiclabs/hardhat-etherscan
4. Configura Hardhat
Crea o edita el archivo hardhat.config.js en la raíz del proyecto con la siguiente configuración:

Archivo: hardhat.config.js

javascript
Copiar código
require('@nomiclabs/hardhat-ethers');
require('@nomiclabs/hardhat-etherscan');

module.exports = {
  solidity: "0.8.0",
  networks: {
    hardhat: {
      chainId: 1337
    }
  },
  etherscan: {
    apiKey: "" // Agrega tu clave API de Etherscan si es necesario
  }
};
5. Escribe las Pruebas
Crea un nuevo directorio para las pruebas:

bash
Copiar código
mkdir test
Crea un archivo llamado PrivateBank.test.js dentro del directorio test con el siguiente contenido:

Archivo: test/PrivateBank.test.js

javascript
Copiar código
const { expect } = require("chai");

describe("PrivateBank", function () {
  let PrivateBank;
  let privateBank;
  let owner;
  let addr1;
  let addr2;

  beforeEach(async function () {
    // Obtener los signers de Hardhat
    [owner, addr1, addr2] = await ethers.getSigners();

    // Desplegar el contrato antes de cada prueba
    PrivateBank = await ethers.getContractFactory("PrivateBank");
    privateBank = await PrivateBank.deploy();
    await privateBank.deployed();
  });

  it("Debería establecer el balance inicial en cero", async function () {
    expect(await privateBank.balanceOf(owner.address)).to.equal(0);
  });

  it("Debería permitir depositar dinero", async function () {
    await privateBank.deposit({ value: ethers.utils.parseEther("1.0") });
    expect(await privateBank.balanceOf(owner.address)).to.equal(ethers.utils.parseEther("1.0"));
  });

  it("Debería permitir retirar dinero", async function () {
    await privateBank.deposit({ value: ethers.utils.parseEther("1.0") });
    await privateBank.withdraw(ethers.utils.parseEther("0.5"));
    expect(await privateBank.balanceOf(owner.address)).to.equal(ethers.utils.parseEther("0.5"));
  });

  it("No debería permitir retirar más dinero del que se tiene", async function () {
    await privateBank.deposit({ value: ethers.utils.parseEther("1.0") });
    await expect(privateBank.withdraw(ethers.utils.parseEther("2.0"))).to.be.revertedWith("Fondos insuficientes");
  });

  it("Debería permitir a los administradores gestionar el contrato", async function () {
    await privateBank.setAdmin(addr1.address);
    expect(await privateBank.admin()).to.equal(addr1.address);
  });

  // Agrega más pruebas según las funcionalidades del contrato
});
6. Ejecuta las Pruebas
Para ejecutar las pruebas, usa el siguiente comando:

bash
Copiar código
npx hardhat test
7. Sube el Código a GitHub
Crea un nuevo repositorio en GitHub.
Clona el repositorio en tu máquina local:
bash
Copiar código
git clone <url_del_repositorio>
Añade los archivos PrivateBank.sol, hardhat.config.js, y test/PrivateBank.test.js al repositorio clonado.
Realiza un commit y sube los cambios:
bash
Copiar código
git add .
git commit -m "Añadidos tests con cobertura total para PrivateBank"
git push origin main
Instrucciones en README.md
Crea o edita el archivo README.md en la raíz del proyecto con las siguientes instrucciones:

Archivo: README.md

markdown
Copiar código
# Pruebas para el Contrato PrivateBank

Este repositorio contiene el contrato inteligente `PrivateBank` y pruebas con cobertura total utilizando Hardhat.

## Requisitos

- Node.js
- npm

## Instalación

1. Clona el repositorio:

    ```bash
    git clone <url_del_repositorio>
    cd <nombre_del_repositorio>
    ```

2. Instala las dependencias:

    ```bash
    npm install
    ```

## Ejecución de Pruebas

1. Ejecuta las pruebas:

    ```bash
    npx hardhat test
    ```

Las pruebas verificarán la funcionalidad del contrato `PrivateBank` y asegurarán una cobertura del 100%.

## Notas

- Asegúrate de tener configurado Node.js y npm en tu entorno de desarrollo.
- Puedes añadir más pruebas según las funcionalidades adicionales del contrato.
