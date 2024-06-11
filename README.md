# propria-ciptomeda-py
criar a sua própria criptomoeda usando Python

Essencialmente, a blockchain é um banco de dados público que documenta e autentica, de maneira irreversível, a posse e a transmissão de ativos digitais. Moedas digitais, como o Bitcoin e o Ethereum, são baseadas nesse conceito. A blockchain é uma tecnologia maravilhosa, que você pode usar para transformar suas aplicações.

Atualmente, governos, organizações e indivíduos têm usado a tecnologia da blockchain para criar suas próprias criptomoedas e para evitar ficar pra trás. Notavelmente, quando o Facebook propôs sua própria criptomoeda, chamada de Libra, o anúncio gerou muita repercussão ao redor do mundo.

E se você pudesse também fazer como eles e criar a sua própria versão de criptomoeda?

Eu pensei sobre isso e decidi desenvolver um algoritmo que cria uma criptomoeda.

Decidi chamá-la de: fccCoin.

Neste tutorial, vou ilustrar passo a passo o processo usado para construir essa moeda digital (usando conceitos de programação orientada a objetos da linguagem Python para fazer isso).

Aqui está o modelo básico de algoritmo para blockchain usado para criar a  fccCoin:

class Block:

    def __init__():

    #Primeiro bloco da classe

        pass
    
    def calculate_hash():
    
    #Calcula o hash para cada bloco
        
    
class BlockChain:
    
    def __init__(self):
     # Método Construtor
    pass
    
    def construct_genesis(self):
        # Constrói o bloco inicial
        pass

    def construct_block(self, proof_no, prev_hash):
        # Constrói um novo bloco e adiciona-o à cadeia de blocos.
        pass

    @staticmethod
    def check_validity():
        # Verifica se a blockchain é válida.
        pass

    def new_data(self, sender, recipient, quantity):
        # Adiciona uma nova transação ao registro de transações
        pass

    @staticmethod
    def construct_proof_of_work(prev_proof):
        # Protege a blockchain de ataques
        pass
   
    @property
    def last_block(self):
        # Retorna o último bloco da cadeia
        return self.chain[-1]

Agora, deixe-me explicar como fazer...

1. Criando a primeira classe "Block"
Uma blockchain é uma cadeia de blocos que são unidos uns aos outros (soa familiar, não soa?).

Essa cadeia de blocos é construída de tal forma que, se um dos blocos for adulterado, toda a cadeia se torna inválida.

Aplicando o conceito acima, criamos a primeira classe Block.

import hashlib
import time

class Block:

    def __init__(self, index, proof_no, prev_hash, data, timestamp=None):
        self.index = index
        self.proof_no = proof_no
        self.prev_hash = prev_hash
        self.data = data
        self.timestamp = timestamp or time.time()

    @property
    def calculate_hash(self):
        block_of_string = "{}{}{}{}{}".format(self.index, self.proof_no,
                                              self.prev_hash, self.data,
                                              self.timestamp)

        return hashlib.sha256(block_of_string.encode()).hexdigest()

    def __repr__(self):
        return "{} - {} - {} - {} - {}".format(self.index, self.proof_no,
                                               self.prev_hash, self.data,
                                               self.timestamp)
Como podemos ver no código acima, foi definido o método __init__(), que será executado quando o bloco for instanciado, assim como em qualquer outra classe em Python.

Foram definidos os seguintes parâmetros para a inicialização da função:

self — refere-se à instância do bloco de classe, tornando possível acessar os métodos e atributos associados a ela;
index — mantém o controle da posição do bloco dentro da cadeia de blocos (blockchain);
proof_no — é o número produzido durante a criação de um bloco (chamada de mineração);
prev_hash — refere-se ao hash do bloco anterior da cadeia;
data — mantém o registro de todas as operações feitas, com informações como a quantidade comprada;
timestamp — possui a data e a hora em que as transações aconteceram.
O segundo método da classe, calculate_hash, gerará um hash para os blocos que usarem os parâmetros descritos acima. O módulo SHA-256 foi importado no projeto para auxiliar na obtenção de hashes dos blocos.

Depois que os parâmetros forem passados para o algoritmo de hash, a função então retornará uma string de 256 bits representando o conteúdo do bloco.

É assim que a segurança é garantida em blockchains. Cada bloco terá um hash e esse hash fará referência a um hash do bloco anterior.

Dessa forma, se alguém tentar comprometer qualquer bloco da cadeia, os outros blocos terão hashes inválidos, o que fará com que a cadeia inteira seja inválida.

Até o momento, um bloco tem essa aparência:

{
    "index": 2,
    "proof": 21,
    "prev_hash": "6e27587e8a27d6fe376d4fd9b4edc96c8890346579e5cbf558252b24a8257823",
    "transactions": [
        {'sender': '0', 'recipient': 'Quincy Larson', 'quantity': 1}
    ],
    "timestamp": 1521646442.4096143
}
2. Construindo a classe Blockchain
A ideia principal por trás da blockchain, assim como o próprio nome sugere, envolve encadear vários blocos uns aos outros.

Portanto, será construída uma classe Blockchain, que será muito útil no gerenciamento do trabalho de toda a cadeia. Será nela que a maior parte do trabalho será feito.

A classe Blockchain terá vários métodos auxiliares para desempenhar várias funções na cadeia de blocos.

Será explicado o que cada um desses métodos fará na classe.

a. Método construtor
Esse método é o responsável pela inicialização da classe.

class BlockChain:

    def __init__(self):
        self.chain = []
        self.current_data = []
        self.nodes = set()
        self.construct_genesis()
Aqui estão as atribuições do método construtor:

self.chain — variável que mantém todos os blocos;
self.current_data — variável que controla todas as transações feitas nesse bloco;
self.construct_genesis() — método que cuidará da criação do bloco inicial.
b. Construindo o bloco inicial
A blockchain precisa do método construct_genesis para criar o bloco inicial da cadeia. Normalmente, nas blockchains, esse bloco é especial, pois ele simboliza o início da cadeia.

Agora, o bloco inicial será construído com valores padrão passados ao método construct_block.

Foi definido o valor zero para proof_no e prev_hash, mas você pode definir o valor que quiser para eles.

def construct_genesis(self):
    self.construct_block(proof_no=0, prev_hash=0)


def construct_block(self, proof_no, prev_hash):
    block = Block(
        index=len(self.chain),
        proof_no=proof_no,
        prev_hash=prev_hash,
        data=self.current_data)
    self.current_data = []

    self.chain.append(block)
    return block
c. Criando outros blocos

O método construct_block é usado para criar outros blocos na blockchain.

Adiante, vemos os valores dos atributos desse método:

index — representa o tamanho da blockchain;
proof_nor e prev_hash — o método que chama o construtor passará esses parâmetros;
data — aqui temos o registro das transações que não foram incluídas em nenhum bloco da cadeia;
self.current_data — essa variável é usada para reiniciar a lista de transações nesse bloco. Se um bloco foi construído e transações forem registradas nele, a lista é reiniciada para se ter certeza que transações futuras serão adicionadas a essa variável;
self.chain.append() — esse método adiciona os blocos recém-construídos à cadeia;
return — por último, temos um objeto do tipo bloco que é retornado.
d. Verificando a validade

Para o método check_validity, é importante verificar a integridade da blockchain e ter certeza de que não há quaisquer anomalias nela.

Como mencionado anteriormente, os hashes são essenciais para a segurança da blockchain. Por isso, qualquer mudança no bloco gerará um hash completamente novo.

Portanto, esse método check_validity usa um if  para verificar se o hash de cada bloco está correto.

Ele também verifica se cada bloco aponta corretamente para o bloco anterior, comparando o valor dos hashes. Se tudo estiver correto, ele retorna verdadeiro. Caso contrário, retorna falso.

@staticmethod
def check_validity(block, prev_block):
    if prev_block.index + 1 != block.index:
        return False

    elif prev_block.calculate_hash != block.prev_hash:
        return False

    elif not BlockChain.verifying_proof(block.proof_no, prev_block.proof_no):
        return False

    elif block.timestamp <= prev_block.timestamp:
        return False

    return True
e. Adicionando dados de transações

O método new_data é usado para adicionar dados de transações a um bloco. É um método muito simples e aceita três parâmetros: (dados do remetente, do recebedor e quantidade) e adiciona isso aos dados da transação na lista self.current_data.

No momento em que um bloco é criado, essa lista é alocada a esse bloco e reiniciada, como foi explicado no método construct_block.

Uma vez que os dados da transação são colocados na lista, a posição do próximo bloco é retornada.

Essa posição é calculada adicionando-se 1 à posição do bloco atual (que é sempre o último da blockchain). Esses dados auxiliarão o usuário no envio de transações futuras.

def new_data(self, sender, recipient, quantity):
    self.current_data.append({
        'sender': sender,
        'recipient': recipient,
        'quantity': quantity
    })
    return True


f. Adicionando a prova de trabalho
Prova de trabalho (texto em inglês) é um conceito que ajuda a blockchain a evitar abusos. De maneira simplificada, o objetivo da prova de trabalho é identificar um número que resolva um problema após algum processamento computacional.

Se a dificuldade desse processamento for alta, o uso de spams e tentativas de fraude serão desencorajadas na blockchain.

Na blockchain que estamos criando, foi usado um algoritmo simples para desencorajar pessoas a minerar ou a criar blocos de maneira muito fácil.

@staticmethod
def proof_of_work(last_proof):
    '''este algoritmo simples identifica um número f' tal que hash(ff') contenha 4 zeros à esquerda
         f é o f' anterior
         f' é a nova prova
        '''
    proof_no = 0
    while BlockChain.verifying_proof(proof_no, last_proof) is False:
        proof_no += 1

    return proof_no


@staticmethod
def verifying_proof(last_proof, proof):
    #verificando a prova: hash(last_proof, proof) contém 4 zeros à esquerda?

    guess = f'{last_proof}{proof}'.encode()
    guess_hash = hashlib.sha256(guess).hexdigest()
    return guess_hash[:4] == "0000"

g. Obtendo o último bloco
Finalmente, o método latest_block é um método auxiliar que ajuda na obtenção do último bloco da blockchain. Lembre-se de que o último bloco, na verdade, é o bloco atual da blockchain.

@property
    def latest_block(self):
        return self.chain[-1]
Vamos resumir tudo até aqui
Aqui está o código completo para criação da criptomoeda fccCoin.

Você também pode dar uma olhada no código através desse repositório no Github.

import hashlib
import time


class Block:

    def __init__(self, index, proof_no, prev_hash, data, timestamp=None):
        self.index = index
        self.proof_no = proof_no
        self.prev_hash = prev_hash
        self.data = data
        self.timestamp = timestamp or time.time()

    @property
    def calculate_hash(self):
        block_of_string = "{}{}{}{}{}".format(self.index, self.proof_no,
                                              self.prev_hash, self.data,
                                              self.timestamp)

        return hashlib.sha256(block_of_string.encode()).hexdigest()

    def __repr__(self):
        return "{} - {} - {} - {} - {}".format(self.index, self.proof_no,
                                               self.prev_hash, self.data,
                                               self.timestamp)


class BlockChain:

    def __init__(self):
        self.chain = []
        self.current_data = []
        self.nodes = set()
        self.construct_genesis()

    def construct_genesis(self):
        self.construct_block(proof_no=0, prev_hash=0)

    def construct_block(self, proof_no, prev_hash):
        block = Block(
            index=len(self.chain),
            proof_no=proof_no,
            prev_hash=prev_hash,
            data=self.current_data)
        self.current_data = []

        self.chain.append(block)
        return block

    @staticmethod
    def check_validity(block, prev_block):
        if prev_block.index + 1 != block.index:
            return False

        elif prev_block.calculate_hash != block.prev_hash:
            return False

        elif not BlockChain.verifying_proof(block.proof_no,
                                            prev_block.proof_no):
            return False

        elif block.timestamp <= prev_block.timestamp:
            return False

        return True

    def new_data(self, sender, recipient, quantity):
        self.current_data.append({
            'sender': sender,
            'recipient': recipient,
            'quantity': quantity
        })
        return True

    @staticmethod
    def proof_of_work(last_proof):
        '''este algoritmo simples identifica um número f', de modo que hash(ff') contenha 4 zeros à esquerda
         f é o f' anterior
         f' é a nova prova
        '''
        proof_no = 0
        while BlockChain.verifying_proof(proof_no, last_proof) is False:
            proof_no += 1

        return proof_no

    @staticmethod
    def verifying_proof(last_proof, proof):
        #verificando a prova: hash(last_proof, proof) contém 4 zeros à esquerda?

        guess = f'{last_proof}{proof}'.encode()
        guess_hash = hashlib.sha256(guess).hexdigest()
        return guess_hash[:4] == "0000"

    @property
    def latest_block(self):
        return self.chain[-1]

    def block_mining(self, details_miner):

        self.new_data(
            sender="0",  #implica que esse nó criou um outro bloco
            receiver=details_miner,
            quantity=
            1,  #criar um bloco (ou identificar o número da prova) recebe um valor 1
        )

        last_block = self.latest_block

        last_proof_no = last_block.proof_no
        proof_no = self.proof_of_work(last_proof_no)

        last_hash = last_block.calculate_hash
        block = self.construct_block(proof_no, last_hash)

        return vars(block)

    def create_node(self, address):
        self.nodes.add(address)
        return True

    @staticmethod
    def obtain_block_object(block_data):
        #obtains block object from the block data

        return Block(
            block_data['index'],
            block_data['proof_no'],
            block_data['prev_hash'],
            block_data['data'],
            timestamp=block_data['timestamp'])


Agora, vamos testar o código e ver se ele funciona.

blockchain = BlockChain()

print("***Mineração de fccCoin prestes a começar***")
print(blockchain.chain)

last_block = blockchain.latest_block
last_proof_no = last_block.proof_no
proof_no = blockchain.proof_of_work(last_proof_no)

blockchain.new_data(
    sender="0",  #implica que esse nó criou um outro bloco
    recipient="Quincy Larson",  #let's send Quincy some coins!
    quantity=
    1,  #criar um bloco (ou identificar o número da prova) recebe um valor 1
)

last_hash = last_block.calculate_hash
block = blockchain.construct_block(proof_no, last_hash)

print("***Mineração de fccCoin feita com sucesso***")
print(blockchain.chain)

Funciona!

Aqui está o resultado da mineração que fizemos:

***Mining fccCoin about to start***
[0 - 0 - 0 - [] - 1566930640.2707076]
***Mining fccCoin has been successful***
[0 - 0 - 0 - [] - 1566930640.2707076, 1 - 88914 - a8d45cb77cddeac750a9439d629f394da442672e56edfe05827b5e41f4ba0138 - [{'sender': '0', 'recipient': 'Quincy Larson', 'quantity': 1}] - 1566930640.5363243]

Conclusões
Agora, temos uma criptomoeda!

Esse foi o tutorial de como criar sua própria criptomoeda usando Python.

Porém, é preciso notar que esse tutorial apenas demonstra os conceitos básicos e que serve apenas pra você molhar os pés nessa onda de inovação trazida pela tecnologia da blockchain.

Se essa moeda fosse lançada como está, ela poderia não atender às demandas atuais de mercado por estabilidade, segurança e facilidade de uso.

Portanto, ela pode ser melhorada, adicionando-se novos recursos para aumentar as capacidades de mineração e o envio de transações financeiras (texto em inglês).

Entretanto, é um bom ponto de partida se você decidir ficar famoso no maravilhoso mundo das criptomoedas.
