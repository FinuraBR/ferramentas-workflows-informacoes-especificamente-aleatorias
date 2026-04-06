# Tabela de Tipos de Dados e Limites

> Referência para uso em Cheat Engine, reverse engineering e programação geral.
> Valores fracionários usam ponto (.) como separador decimal.

---

## Tipos Inteiros — Sem Sinal (Unsigned)

| Tipo de Dado               | Tamanho | Valor Mínimo | Valor Máximo        | Aceita "Inf"? | Overflow         |
|----------------------------|---------|--------------|---------------------|---------------|------------------|
| **Byte** (8-bit unsigned)  | 1 Byte  | 0            | 255                 | Não           | Vira 0           |
| **Word** (16-bit unsigned) | 2 Bytes | 0            | 65.535              | Não           | Vira 0           |
| **DWORD** (32-bit unsigned)| 4 Bytes | 0            | 4.294.967.295       | Não           | Vira 0           |
| **QWORD** (64-bit unsigned)| 8 Bytes | 0            | 18.446.744.073.709.551.615 | Não  | Vira 0           |

---

## Tipos Inteiros — Com Sinal (Signed)

| Tipo de Dado                    | Tamanho | Valor Mínimo               | Valor Máximo               | Aceita "Inf"? | Overflow               |
|---------------------------------|---------|----------------------------|----------------------------|---------------|------------------------|
| **Int8** (8-bit signed)         | 1 Byte  | -128                       | 127                        | Não           | Wraps para extremo oposto |
| **Short / Int16** (16-bit signed)| 2 Bytes | -32.768                   | 32.767                     | Não           | Wraps para extremo oposto |
| **Int / Int32** (32-bit signed) | 4 Bytes | -2.147.483.648             | 2.147.483.647              | Não           | Wraps para extremo oposto |
| **Int64** (64-bit signed)       | 8 Bytes | -9.223.372.036.854.775.808 | 9.223.372.036.854.775.807  | Não           | Wraps para extremo oposto |

---

## Tipos de Ponto Flutuante (Floating Point)

| Tipo de Dado                        | Tamanho  | Valor Mínimo Positivo | Valor Máximo Positivo | Aceita "Inf"? | +Inf (Hex)       | -Inf (Hex)       | NaN (Hex)        | Precisão Decimal |
|-------------------------------------|----------|-----------------------|-----------------------|---------------|------------------|------------------|------------------|------------------|
| **Float** (Single Precision / 32-bit) | 4 Bytes | ~1.175e-38            | ~3.402e+38            | Sim           | 7F800000         | FF800000         | 7FC00000         | ~7 dígitos       |
| **Double** (Double Precision / 64-bit)| 8 Bytes | ~2.225e-308           | ~1.797e+308           | Sim           | 7FF0000000000000 | FFF0000000000000 | 7FF8000000000000 | ~15 dígitos      |

---

## Tipos de Texto e Caractere

| Tipo de Dado              | Tamanho         | Valor Mínimo | Valor Máximo | Encoding Padrão | Observação                        |
|---------------------------|-----------------|--------------|--------------|-----------------|-----------------------------------|
| **Char** (ASCII)          | 1 Byte          | 0 (NUL)      | 127          | ASCII           | Caracteres básicos (a-z, 0-9...) |
| **Char** (Extended ASCII) | 1 Byte          | 0            | 255          | ISO-8859 / CP   | Inclui acentos e símbolos        |
| **WChar** (Wide Char)     | 2 Bytes         | 0            | 65.535       | UTF-16 LE       | Padrão no Windows (Unicode)      |
| **String** (ASCII)        | N × 1 Byte      | —            | —            | ASCII / UTF-8   | Terminada em \0 (null-terminated)|
| **WString** (Unicode)     | N × 2 Bytes     | —            | —            | UTF-16 LE       | Terminada em \0\0                |

---

## Tipos Lógicos e Especiais

| Tipo de Dado        | Tamanho          | Valores Possíveis       | Observação                                     |
|---------------------|------------------|-------------------------|------------------------------------------------|
| **Bool** (C/C++)    | 1 Byte (típico)  | 0 (false) / 1 (true)   | Qualquer valor != 0 é tratado como true        |
| **Bool** (4 Bytes)  | 4 Bytes          | 0 (false) / 1 (true)   | Comum em engines (ex: Unreal Engine)           |
| **Pointer 32-bit**  | 4 Bytes          | 0x00000000              | 0xFFFFFFFF | Endereço de memória em processos 32-bit |
| **Pointer 64-bit**  | 8 Bytes          | 0x0000000000000000      | 0x7FFFFFFFFFFFFFFF | Endereço de memória em processos 64-bit |
| **Void**            | 0 Bytes          | —                       | Ausência de tipo; usado em ponteiros genéricos |

---

## Representações Hexadecimais Especiais (Float / Double)

| Valor             | Float (Hex) | Double (Hex)         |
|-------------------|-------------|----------------------|
| +Infinito         | 7F800000    | 7FF0000000000000     |
| -Infinito         | FF800000    | FFF0000000000000     |
| NaN (quiet)       | 7FC00000    | 7FF8000000000000     |
| Zero positivo     | 00000000    | 0000000000000000     |
| Zero negativo     | 80000000    | 8000000000000000     |
| 1.0               | 3F800000    | 3FF0000000000000     |
| -1.0              | BF800000    | BFF0000000000000     |
| 0.5               | 3F000000    | 3FE0000000000000     |
| Máximo finito (+) | 7F7FFFFF    | 7FEFFFFFFFFFFFFF     |

---

## Tamanhos por Arquitetura

| Tipo        | x86 (32-bit) | x64 (64-bit) | Observação                          |
|-------------|--------------|--------------|-------------------------------------|
| **int**     | 4 Bytes      | 4 Bytes      | Mesmo tamanho nas duas arquiteturas |
| **long**    | 4 Bytes      | 4 ou 8 Bytes | Depende do compilador e plataforma  |
| **pointer** | 4 Bytes      | 8 Bytes      | Muda com a arquitetura do processo  |
| **size_t**  | 4 Bytes      | 8 Bytes      | Segue o tamanho do ponteiro         |
