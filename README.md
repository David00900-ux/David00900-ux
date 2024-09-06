/ SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SimpleStorage {
    uint256 private data;

    event DataUpdated(uint256 newData);

    function setData(uint256 _data) public {
        data = _data;
        emit DataUpdated(_data);
    }

    function getData() public view returns (uint256) {
        return data;
    }
}

Red de Prueba: Sepolia
Tx Hash: 0x4873528341D33Ec918c7465F244491aCB75Bc95F
Dirección del Contrato: 0x6B24e286ae90148091Ceaeeb59FbCcf432CB5A17
Contract Specs ID: 0x4873528341D33Ec918c7465F244491aCB75Bc95F
