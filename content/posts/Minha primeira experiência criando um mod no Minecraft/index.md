---
date: 2026-04-23T21:17:45-03:00
description: "Eu comecei sem saber nada (só sabia Java), e depois aprendi que eu realmente não sabia de nada (nem de Java)."
# image: ""
lastmod: 2026-04-25
showTableOfContents: true
tags: ["Minecraft", "Neoforge", "Java", "Nephus"]
title: "Minha primeira experiência criando um mod no Minecraft"
type: "post"
---

![Banner](banner.png)

Esse é meu primeiro post aqui no blog, e para começarmos, porque não passar primeiro pelo projeto que estou fazendo atualmente, não é?

Pois bem, eu conheci e jogo Minecraft desde criança quando eu tinha lá meus 8 anos, lá pra época de 2013 para o que eu me recorde, e se tem uma coisa que me chamou atenção no jogo, era principalmente a liberdade e a criatividade dos jogadores em criar diversos modos de jogos, naquele tempo, o Hide-and-Seek era popular e muitos servidores rodavam na versão **1.6.2** ou **1.6.3** e eram muito divertidos.

De lá pra cá, eu comecei a explorar cada vez mais das opções que o jogo oferecia, até chegar no que eu mais amo do jogo até hoje: os mods. Nas versões 1.2.5, 1.5.2, 1.6.4 e em diante, eu testei vários mods que me fizeram colocar o jogo como meu preferido até os dias atuais, como o Digimobs, Galacticraft, Pokecube, IndustrialCraft, Morph, entre outros.

Eu ainda tentei aprender Java, lá em 2014, mas assim, eu tinha 8 anos, e aprender algo para diversão, principalmente uma linguagem de programação relativamente complexa para depois aprender um ecossistema completo de modificações para um jogo... Certamente não é toda criança que consegue, e eu prefiri ir fazer um apocalipse no GTA San Andreas mesmo.

Como eu sou estudante de Ciência da Computação e aprendi Java na minha universidade, então recentemente resolvi tentar novamente meu "sonho". Nesse post vou tentar contar minha experiência como eu conseguir e ao mesmo tempo ensinar a você leitor como você pode começar também.

## Por que NeoForge 1.21.1?

Eu falei que eu não ia enrolar mais, mas eu estava mentindo. Eu inicialmente comecei a ideia de fazer o projeto na versão mais recente do Minecraft atualmente (26.1.2), mas durante cada atualização de versão do jogo e também do Modloader, muitos métodos e formas de fazer algo mudam completamente, isso leva tempo para a comunidade aprender e transformar mesmo com a [documentação oficial do projeto](https://docs.neoforged.net/).

Mesmo com o uso de IA, muito do conteúdo que ela se baseava, ainda era mais antigo e ficar consertando as mudanças ia me dar ainda mais trabalho, ou seja, preguiça. Então eu resolvi trocar para uma versão mais estável do jogo, a versão 1.21.1. Querida por muitos, e o último pontapé da comunidade para a maioria dos modders aprenderem e desenvolverem ainda mais o ecossistema das versões mais recentes.

## O que eu precisei antes de começar

> **ATENÇÃO:** Esse artigo não é um tutorial de Java, e é necessário possuir algum conhecimento na linguagem, se você não possui, muito do conteúdo aqui vai parecer que foi retirado de um livro de magia de Harry Potter.

Para começarmos, eu vou dar um overview de tudo que precisamos:
+ [IntelliJ IDEA](https://www.jetbrains.com/idea/)
+ [Minecraft Development Extension](https://plugins.jetbrains.com/plugin/8327-minecraft-development)
+ [Java 21](https://adoptium.net/pt-BR/temurin/releases?version=21)

Não é necessário nenhuma configuração específica de cada uma, apenas instalar. Com tudo isso já instalado e configurado, podemos seguir em frente.

## Gerando o projeto

Para evitar ter que configurar tudo manualmente que é extremamente chato, o NeoForged disponibiliza um [gerador de projetos](https://neoforged.net/mod-generator/) que já cria toda a estrutura inicial pronta pra uso.

![NeoforgeModGenerator](images/modgenerator.png)

Ao abrir o site, você vai ver um formulário com algumas opções. Aqui estão as principais que eu utilizei:

+ **Mod Name:** N.E.P.H.U.S (quem conhece, sabe.)
+ **Mod ID:** nephus
+ **Package Name:** com.nephus.mod
+ **Minecraft Version:** 1.21.1
+ **Gradle Plugin:** ModDevGradle

Pra quê serve cada coisa? Não importa e não é nada tão importante, se quiser saber mais, apenas pesquise mais sobre para aprender, mas uma dica: o Mod ID será muito utilizado, então mantenha um nome simples. Após fazer tudo, só baixar o arquivo .zip, extrair e importar o diretório para o IntelliJ.

## Entendendo a estrutura do projeto

Quando você abre o projeto pela primeira vez, dá um susto. Parece muita coisa, mas boa parte é só estrutura padrão.

Aqui estão os pontos que mais importam no início:

+ `src/main/java`: onde fica o código Java do mod.
+ `src/main/resources`: onde ficam arquivos de dados, texturas, modelos, traduções e outros metadados.
+ `build.gradle`: configuração de build, dependências e versão.
+ `gradle.properties`: propriedades do projeto, como versões e nome do mod.

No começo, eu fiquei tentando entender tudo de uma vez e isso foi um erro. O que realmente funcionou pra mim foi: pegar uma coisa pequena e fazer ela funcionar de ponta a ponta.

## Mod técnico é difícil, né? Na verdade, é bem pior do que eu pensava...

Como o post é meu, eu vou focar no que eu já fiz de um mod técnico, uma bateria de energia, e para isso vamos criar um item chamado `EnergyBattery`.

### Criando o Item `EnergyBattery`

![Item Package](images/itempackage.png)

Para criarmos o Item, primeiramente, eu tive que criar um package chamado `item` no diretório principal e em seguida, criei uma classe chamada `EnergyBattery`.

Nela, extendemos nossa classe através do objeto `Item`, um objeto padrão do jogo que usamos como base para todo e qualquer item que criarmos. Após isso, eu sobrescrevi alguns métodos da classe principal e o resultado ficou assim:

```java
package com.aiard0.nephus.item;

// Classe base de item do Minecraft
import net.minecraft.world.item.Item;
// Representa uma instância do item no inventário/mundo
import net.minecraft.world.item.ItemStack;
// Componente customizado onde a energia é armazenada no item
import com.aiard0.nephus.registry.ModDataComponents;

// Nosso item de bateria de energia
public class EnergyBattery extends Item {

    // Capacidade máxima da bateria (energia total suportada)
    private static final int capacity = 5000;

    // Construtor padrão do item
    public EnergyBattery(Properties properties) {
        // Repassa as propriedades para a classe Item (stack size, raridade, etc.)
        super(properties);
    }

    // Getter estático para reutilizar a capacidade em outros pontos do código
    public static int getCapacity() {
        return capacity;
    }

    @Override
    public boolean isBarVisible(ItemStack stack) {
        // Sempre exibe a barrinha abaixo do item no inventário
        return true;
    }

    @Override
    public int getBarWidth(ItemStack stack) {
        // Lê a energia armazenada no Data Component; se não existir, usa 0
        int stored = stack.getOrDefault(ModDataComponents.ENERGY_COMPONENT.get(), 0);

        // Sem energia = sem barra
        if (stored <= 0) {
            return 0;
        }
        // Com energia cheia = barra completa (13 é o máximo no Minecraft)
        if (stored >= getCapacity()) {
            return 13;
        }

        // Calcula o tamanho proporcional da barra entre 0 e 13
        return stored * 13 / getCapacity();
    }
}
```

Ai você me pergunta: pra que serve esse código? E eu te respondo: leia. Simples assim, mas pra resumir o funcionamento:

+ `EnergyBattery(Properties properties)`: cria o item e aplica as propriedades base definidas no registro (aqui não tem nenhuma).
+ `getCapacity()`: retorna a capacidade máxima da bateria (neste caso, 5000).
+ `isBarVisible(ItemStack stack)`: força a barra de energia a aparecer no item no inventário.
+ `getBarWidth(ItemStack stack)`: calcula o tamanho da barra com base na energia armazenada (de 0 a 13).

Agora que todo o scaffolding da nossa `EnergyBattery` está feito, existe algo essencial que ainda não expliquei: o **Data Component**.

### Criando um `Data Component` de energia

De forma rápida, o Data Component é o lugar onde a gente guarda dados customizados do item de forma oficial no Minecraft (). No nosso caso, ele guarda a energia da bateria.

Ele é extremamente necessário para qualquer item que manipule energia. Sem ele, a barra até pode existir no código, mas não teria um valor confiável e individual para ler e mostrar. Com ele, cada `ItemStack` da bateria consegue salvar e recuperar a própria carga, e por isso o método `getBarWidth` funciona do jeito certo.

> Para os burros que nem eu: isso garante que cada bateria que vocẽ tiver no inventário vai ter sua própria energia, simples e mágico, não é?

Pois bem, eu criei um package `registry`, onde concentramos vários registros que devem ser anexados a inicialização do jogo, e dentro do mesmo, criei a classe `ModDataComponents`, aqui está o resultado:

```java
package com.aiard0.nephus.registry;

// Classe principal do mod (usada para acessar o MODID)
import com.aiard0.nephus.Nephus;
// Codec usado para serializar/desserializar tipos simples (aqui, Integer)
import com.mojang.serialization.Codec;
// Tipo base de um Data Component
import net.minecraft.core.component.DataComponentType;
// Registro global de tipos de componentes de dados do Minecraft
import net.minecraft.core.registries.Registries;
// Codec de rede para sincronização via pacote
import net.minecraft.network.codec.ByteBufCodecs;
// Barramento de eventos do mod (onde registramos os componentes)
import net.neoforged.bus.api.IEventBus;
// Referência "adiada" para o objeto registrado
import net.neoforged.neoforge.registries.DeferredHolder;
// Sistema de registro seguro do NeoForge
import net.neoforged.neoforge.registries.DeferredRegister;

// Classe responsável por registrar todos os Data Components do mod
public class ModDataComponents {

    // Cria o registrador específico de Data Components do mod "nephus"
    private static final DeferredRegister.DataComponents DATA_COMPONENTS =
            DeferredRegister.createDataComponents(Registries.DATA_COMPONENT_TYPE, Nephus.MODID);

    // Método chamado no bootstrap do mod para efetivar os registros no EventBus
    public static void register(IEventBus modEventBus) {
        DATA_COMPONENTS.register(modEventBus);
    }

    // Registra um componente chamado "energy" com tipo Integer
    // Esse componente é o valor que guarda a energia de cada ItemStack da bateria
    public static final DeferredHolder<DataComponentType<?>, DataComponentType<Integer>> ENERGY_COMPONENT =
            DATA_COMPONENTS.registerComponentType("energy", builder ->
                    builder
                // Permite salvar esse valor em disco (mundo/inventário)
                            .persistent(Codec.INT)
                // Permite sincronizar esse valor entre servidor e cliente
                            .networkSynchronized(ByteBufCodecs.INT)
            );
}
```

Acredita que esse código pode ser encontrado de maneira pura (sem alterações complexas) em vários mods grandes por aí? Eu sei disso porque tive que entender pra que servia e como implementar copiando dos outros né...

Mesmo o código estando comentado, aqui está o resumo da importância de cada um:

+ `ModDataComponents`: É o "catálogo" dos componentes de dados do mod. Sempre que eu precisar criar outro dado customizado (por exemplo, carga, modo, nível etc.), é aqui que vou registrar.
+ `DATA_COMPONENTS`: É um registrador especializado do NeoForge para `DataComponentType`. Ele garante que o registro aconteça na hora correta da inicialização do jogo.
+ `register(IEventBus modEventBus)`: É um método conecta esse registrador ao ciclo de vida do mod. Se esse método não for chamado na classe principal, o componente simplesmente não existe em runtime, logo, não vai funcionar nada.
+ `ENERGY_COMPONENT`: É o componente em si, identificado internamente pelo nome `energy`. Esse nome é a chave usada pelo jogo para localizar e armazenar o valor.
+ `.persistent(Codec.INT)`: Isso diz ao Minecraft para salvar esse valor no disco (mundo/inventário). Sem isso, a carga poderia se perder ao fechar e abrir o jogo.
+ `.networkSynchronized(ByteBufCodecs.INT)`: Serve para sincronizar o valor entre servidor e cliente. Sem essa parte, o valor poderia ficar diferente entre os lados, gerando barra errada ou comportamento inconsistente.

Existem muitos outros tipos de `Data Component`, além de podermos criar nossos próprios, mas não é o importante agora porque só precisamos do de energia.

Essa foi uma das partes que mais me confundiu para aprender, eu simplesmente achava que só usariamos uma API pronta, chamava no item que criamos e cabô, tá feito, mas não, a gente tem que criar uma classe para dizer que a gente VAI usar energia e ele entende "Ok, já vai avisando 👍". É simplesmente impressionante como o ser humano é capaz de facilitar tantas coisas ao mesmo tempo que é capaz de dificultar também... Já dizia Henry David Thoreau:

> _"Nossas invenções costumam ser brinquedos bonitos, que prendem nossa atenção de coisas sérias. São apenas meios melhorados para um fim não melhorado._" - Essa frase foi patrocinada pelo Gemini.

Enfim, agora conseguimos fazer a parte necessária para preencher o código da nossa `EnergyBattery`, mas se iniciarmos o jogo para testar nosso mod, veremos que o item nem se quer aparecer no jogo, pois não definimos ID nem registramos ele na inicialização, só fizemos criar classes para o item em si e persistência dos dados.

A esse ponto do post, eu provavelmente já perdi a sua atenção, mas vou fingir que você chegou aqui com muito entusiasmo, e vamos para a próxima etapa: fazer o registro da nossa bateria na inicialização, chamado `ModItems`.

### Criando nosso registrador de items, o `ModItems`

Ainda dentro do package `registry`, criamos a classe `ModItems`, nela configuramos e adicionamos os itens que criamos para que os mesmos possam ser inicializados no jogo e assim a gente possa segurar, usar, etc. Acho que já deu pra entender, então continuemos com o código:

```java
package com.aiard0.nephus.registry;

// Classe principal do mod (usada para pegar o MODID)
import com.aiard0.nephus.Nephus;
// Classe do nosso item customizado (no caso, nossa EnergyBattery)
import com.aiard0.nephus.item.EnergyBattery;
// Tipo base de item do Minecraft (ainda não percebeu que esse troço aqui vai estar em todo lugar?)
import net.minecraft.world.item.Item;
// Referência adiada para o item registrado
import net.neoforged.neoforge.registries.DeferredItem;
// Sistema de registro seguro do NeoForge
import net.neoforged.neoforge.registries.DeferredRegister;

// Classe responsável pelo registro de todos os itens do mod
public class ModItems {

    // Cria o registrador de itens vinculado ao MODID do mod
    // Tudo que for item do mod passa por esse registrador
    public static final DeferredRegister.Items ITEMS = DeferredRegister.createItems(Nephus.MODID);

    // Registra o item "energy_battery" no jogo
    // "energy_battery" é o ID interno do item (namespaced com nephus:energy_battery)
    // EnergyBattery::new é a fábrica usada para instanciar o item
    // O retorno é um DeferredItem, que só vira o Item real quando o registro é finalizado
    public static final DeferredItem<Item> ENERGY_BATTERY = ITEMS.registerItem(
            "energy_battery",
            EnergyBattery::new
    );

}
```

Mesmo com o código comentado, aqui vai DE NOVO um resumo direto da importância de cada parte caso você não tenha entendido ou prestado atenção aos _maravilhosos_ comentários:

+ `ModItems`: é a classe onde centralizamos o registro de todos os itens do mod.
+ `ITEMS`: é o registrador de itens vinculado ao `MODID`; sem ele, nenhum item seu entra no ciclo de registro.
+ `ENERGY_BATTERY`: é a referência do item registrado, usada depois em receitas, eventos e outras integrações.
+ `"energy_battery"`: é o ID interno do item no jogo, que vira `nephus:energy_battery`.
+ `EnergyBattery::new`: define qual classe Java será instanciada quando o item for criado.
+ `DeferredItem<Item>`: mantém o registro seguro durante a inicialização, evitando acessar o item antes da hora.

Se você não entendeu ainda, não espere que eu magicamente apareça na sua casa e te ensine, faça que nem um Júnior, copie e cole o código. Basicamente tudo aqui serve para criar um registrador geral de itens do nosso mod, e em seguida, adicionamos o nosso item utilizando esse registrador.

É um código curto e simples até, e sua função não foge desse padrão, se quisermos adicionar outro item, como uma bateria maior, depois de criarmos a classe dela só  precisamos copiar o bloco de código dessa inicial, e substituir os relacionamentos com a nova classe que você criar.

Para mais, caso você tenha achado que estava acabando, você está estupidamente enganado, falta tanta coisa ainda que eu não sei nem o que é o próximo tópico.

### Criando o `ModCapabilityEvents` para interoperabilidade

Voltamos novamente para a parte de energia, e dessa vez, aqui trataremos de um ponto muito importante: interoperabilidade. Imagino que você nunca tenha escutado essa palavra na vida se vocẽ possui abaixo de 30 anos de idade, e se a conhece, talvez seja porque seu professor de Geografia tenha atazanado você para fazer um trabalho em que o tema envolvesse essa palavra. (Não tem nada pessoal aqui.)

O `ModCapabilityEvents` é o responsável por permitir que nossa `EnergyBattery` possa se comunicar com itens de outros mods sem precisarmos de configurações manuais pra isso. Quando registrado, isso faz com que nossa bateria possa ser carregada por geradores de energia de vários mods diferentes (Mekanism, Powah, e vários outros mods), ou possamos recarregar alguma máquina que aceite uma bateria, no caso, a nossa.

Você ainda não entendeu a diferença dessa porcaria para o `Data Components`? É mais simples do que parece!

+ **Data Components:** É o cara que guarda o valor da energia dentro do item, individualmente. Deixa salvo a energia de cada item, faz com que não se perca o valor.
+ **Capability:** É o cara que expoe a capacidade dessa bateria para que outros mods possam reconhecer e manipular. Permite que cabos, máquinas e outros mods enxerguem a bateria como algo que pode receber/fornecer energia.

Para contiarmos fazendo acontecer, criamos um novo package chamado `events`, e nele criaremos a classe `ModCapabilityEvents`, aqui adicionamos um método que ficará responsável por registrar as Capabilities de cada item que queremos, mas só temos um né, então obvio qual vai ser. Aqui está o código de como vai ficar:

