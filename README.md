# Bitcoin Script Assembler/Disassembler
[![NPM](https://img.shields.io/npm/v/bscript-assembler.svg)](https://www.npmjs.org/package/bscript-assembler)
[![Build Status](https://travis-ci.com/JBaczuk/bscript-assembler.svg?branch=master)](https://travis-ci.com/JBaczuk/bscript-assembler)

Bitcoin Script Assembler/Disassembler for node or browsers

## Installation

    $ npm i bscript-assembler --save

## CLI Usage
General help:
```
$ bscript -h
Usage: bscript [options] [command]

Options:
  -v, --version                    output the version number
  -h, --help                       output usage information

Commands:
  assemble [options] <raw-script>
  disassemble [options] <asm>
  getopcode <word>
  getword <opcode>
  isvalid <opcode-or-word>
  isdisabled <opcode-or-word>
  describe <opcode-or-word>
```

Help for a specific command, e.g.:
```
$ bscript assemble -h
Usage: assemble [options] <raw-script>

Options:
  -l, --literal-style [style]  Literal Style normal|brackets|prefixed|verbose (default: "normal")
  -e, --encoding [encoding]    Encoding ascii|base64|binary|hex|utf8 (default: "hex")
  -h, --help                   output usage information

$ bscript assemble --literal-style normal 76a914306e2ea1eed91bf66dfe5d94f3957d4ba63bde8488acOP_DUP OP_HASH160 306e2ea1eed91bf66dfe5d94f3957d4ba63bde84 OP_EQUALVERIFY OP_CHECKSIG
```

## Module Usage
You can parse raw hex string into an assembly string using:  

```javascript
> const BScript = require('bscript-assembler')
undefined
> BScript.rawToAsm('a914c664139327b98043febeab6434eba89bb196d1af87', 'hex')
'OP_HASH160 c664139327b98043febeab6434eba89bb196d1af OP_EQUAL'
```

You can parse an assembly string into a raw script hex string using:  

```javascript
> BScript.asmToRaw('OP_HASH160 c664139327b98043febeab6434eba89bb196d1af OP_EQUAL', 'hex')
'a914c664139327b98043febeab6434eba89bb196d1af87'
```

The library also exposes several utility functions for getting information about bscript opcodes.

### Conversion between opcodes and asm terms

```javascript
> BScript.opcodes.opcodeForWord('OP_EQUAL')
135
> BScript.opcodes.wordForOpcode(135)
'OP_EQUAL'
```

### Check if an opcode or asm term exists

```javascript
> bscript.opcodes.opcodeIsValid(135)
true
> bscript.opcodes.opcodeIsValid(256)
false
> bscript.opcodes.wordIsValid('OP_EQUAL')
true
> bscript.opcodes.wordIsValid('OP_CLONE')
false
```

### Check if an opcode or asm term is disabled

```javascript
> bscript.opcodes.opcodeIsDisabled(135)
false
> bscript.opcodes.opcodeIsDisabled(126)
true
> bscript.opcodes.wordIsDisabled('OP_EQUAL')
false
> bscript.opcodes.wordIsDisabled('OP_CAT')
true
```

### Retrieve descriptive information for an opcode or asm term

**Note:** The data is taken from the [bitcoin.it wiki](https://en.bitcoin.it/wiki/Script).

```javascript
> bscript.opcodes.descriptionForOpcode(135)
'Returns 1 if the inputs are exactly equal, 0 otherwise.'
> bscript.opcodes.descriptionForWord('OP_HASH160')
'The input is hashed twice: first with SHA-256 and then with RIPEMD-160.'
> bscript.opcodes.inputDescriptionForOpcode(135)
'x1 x2'
> bscript.opcodes.inputDescriptionForWord('OP_HASH160')
'in'
> bscript.opcodes.outputDescriptionForOpcode(135)
'True / false'
> bscript.opcodes.outputDescriptionForWord('OP_HASH160')
'hash'
```

### API Docs

You can view the the API docs for this project by running `npm run docs:serve` in the project directory and then accessing
the documentation in your browser.  

You can also access the pre-built API documentation as markdown [here](./API.md).  
To prepare the pre-built api docs to be committed after a change is made to the code, run `npm run docs:build`.  The should be done before every release.
