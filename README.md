# O padrão _Factory method_

## Escrito por Victor Souza A.k.a _vhost_

Esse padrão de projeto, tem o conceito de encapsular a criação de objetos em um método de uma classe, onde essa classe vai ter apenas a responsabilidade de instânciar e devolver objetos. Porque utilizamos esse padrão? Note o código abaixo:

```csharp
namespace StreetFighter{ 
    public class Program
    {
        private Character opponent;

        Console.WriteLine("Choose your opponent for start: ");
        string opponentName = Console.ReadLine();

        //several crime here
        if(opponent == "Ryu")
        {
            this.opponent = new Ryu();
        }
        else if(opponent =="Chun li")
        {
            this.opponent = new ChunLi();
        }
        else if(opponent == "Blanka")
        {
            this.opponent = new Blanka();
        }
        //...

        startFight(opponent);

        static void Main(String[] args)
        {
            public startFight(Character opponentOne)
            {
                Console.WriteLine($"Get ready for the next battle, op1 :{opponentOne}, op2: {new RandomOpponent});
            }
        }
    }
}

```
  Temos vários problemas nesse código, ele não está fechado para modificações, toda vez que tivermos que escolher qual objeto devemos instânciar para realizar algo, essa cadeia de if's se repetirá, ( _e trocar por switch case não melhora kkk_ ), e essa classe não está com responsabilidade única, terá um sobrepeso enorme devido as instânciações que ela deverá fazer para executar os métodos que precisarem. Aí que entra o _Factory method_ delegaremos a função de criar objetos e nos devolver a uma classe específica, isso permitirá a escolha de qual objeto instânciar em tempo de execução de uma forma mais flexível, sem ter que replicar essa cadeia de if's. Observe:

```csharp
public abstract class CharacterFactory
{
    public abstract Character GetNewCharacter(Character character);
}
``` 
```csharp
public abstract class Character
{
    public string Name;
    
    public int TotalDamage;

    public string SpecialHit;
}
``` 
```csharp
public class StreetFighterFactory : CharacterFactory
{
    public override Character GetNewCharacter(Character StreetFighterCharacter){
        
        switch(StreetFighterCharacter)
        {
            case "Ryu":
                return new Ryu();
                break;

            case "Chun li":
                return new ChunLi();
                break;
            
            case "Blanka":
                return new Blanka();
                break;
        }
    }
}
``` 
```csharp
public class Ryu : Character
{
    public string Name = "Ryu";
    
    public int TotalDamage = 40.0;

    public string SpecialHit = "Hadouken";
}
```
Repare que agora, nosso código está muito mais flexível, temos uma classe abstrata que representa a nossa _Factory_ onde contém nosso _Factory method_ que recebe um valor do tipo Character, que é outra classe abstrata,
e retorna um objeto de uma classe concreta que herda dessa classe. Com essa implementação, tiramos a responsabilidade de decidir qual objeto instânciar da classe principal, e deixamos que as subclasses de CharacterFactory decidirem, através dos seus _Factory methods_, conseguimos manter o princípio de responsabilidade única para as classes, podemos agora criar várias _Factories_ que podem cuidar de outros _Characters_, se quiséssemos adicionar uma factory para personagens do **Mario**,**Tekken** ou **Sonic**, ao invés de uma cadeia de if's e elses, basta estendermos os comportamentos das nossas classes abstratas. Esse é o conceito, criamos classes abstratas criadoras e classes que representam o produto, criamos implementações concretas, onde seus métodos cuidarão de instânciar e devolver o objeto correto, isso é um _Factory method_. Não podemos evitar o fato de nossos _Factory methods_ crescerem ou serem alterados caso tenha que haver uma instânciação de um novo objeto, ou remoção de uma, mas ao menos estaremos alterando em uma classe respectiva, onde ela só tem um método, que tem apenas uma função, então o trabalho e o risco das alterações são bem menores em relação a if's _hard-coded_. Observe que simples nosso código na **Main** fica, atavés das _Factories_:

```csharp
namespace StreetFighter{ 
    public class Program
    {
        private Character opponent;

        Console.WriteLine("Choose your opponent for start: ");
        string opponentName = Console.ReadLine();

        CharacterFactory streetFighterFactory = new StreetFighterFactory();

        opponent = streetFighterFactory.GetNewCharacter(opponent);
      
        startFight(opponent);

        static void Main(String[] args)
        {
            public startFight(Character opponentOne)
            {
                Console.WriteLine($"Get ready for the next battle, op1 :{opponentOne}, op2: {new RandomOpponent});
            }
        }
    }
}

```
Simplesmente instânciamos nossa _Factory_ e chamamos seu método, garantido nosso objeto correto instânciado, de uma forma muito melhor.

## Final 

Espero que isso lhe ajude a compreender um pouco mais sobre o padrão _Factory method_, tire o máximo proveito do que ele oferece. 

Obrigado a todos os leitores do artigo!