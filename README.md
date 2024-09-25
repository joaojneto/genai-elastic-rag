# genai-elastic-rag
A demo application to create a ChatBot to suggest movies


# Delete the current test index

```sh
DELETE br_text
````

# Create a new index! It's mandatory to create this mapping.
```sh
PUT br_text
{
  "mappings": {
    "properties": {
      "text_embedding.predicted_value": { 
        "type": "dense_vector", 
        "dims": 384 
      },
      "message": { 
        "type": "text" 
      },
      "titulo": {
        "type": "keyword"
      }
    }
  }
}
```

# Create a ingest pipeline to enrich the documents with embeddings data
```sh
PUT _ingest/pipeline/ml-35-small
{
  "processors": [
    {
      "inference": {
        "model_id": ".multilingual-e5-small_linux-x86_64",
        "target_field": "text_embedding",
        "field_map": {
          "message": "text_field"
        }
      }
    }
  ]
}
```

# Create the documents
```sh
PUT br_text/_doc/1
{
  "titulo": "twister",
  "message": "Twisters é uma continuação do longa homônimo de Jan de Bont, lançado em 1996. Desta vez, sob a direção de Lee Isaac Chung (Minari) o filme foca em uma dupla de caçadores de tempestade. Kate Cooper (Daisy Edgar-Jones) é uma ex-caçadores desses fenômenos, mas que acaba sendo atraída de volta às planícies por seu amigo Javi (Anthony Ramos), para testar um novo sistema experimental de rastreamento meteorológico. Nessa missão, ela cruza seu caminho com Tyler Owens (Glen Powell), um ícone das redes sociais que compartilha suas aventuras de caça à tempestade. Conforme a temporada de tempestades se intensifica e acontecimentos aterrorizantes tomam conta, Kate e Tyler, que são concorrentes, se encontram em meio a uma situação nunca vista antes, colocando suas vidas em risco."
}

PUT br_text/_doc/2
{
  "titulo": "divertidamente 2",
  "message": "Divertidamente 2 marca a sequência da famosa história de Riley (Kaitlyn Dias). Com um salto temporal, a garota agora se encontra mais velha, com 13 anos de idade, passando pela tão temida pré-adolescência. Junto com o amadurecimento, a sala de controle mental da jovem também está passando por uma demolição para dar lugar a algo totalmente inesperado: novas emoções. As já conhecidas, Alegria (Amy Poehler), Tristeza (Phyllis Smith), Raiva (Lewis Black), Medo (Tony Hale) e Nojinho (Liza Lapira), que desde quando Riley é bebê, eles predominam a central de controle da garota em uma operação bem-sucedida, tendo algumas falhas no percurso como foi apresentado no primeiro filme. As antigas emoções não têm certeza de como se sentir e com agir quando novos inquilinos chegam ao local, sendo um deles a tão temida Ansiedade (Maya Hawke). Inveja (Ayo Edebiri), Tédio (Adèle Exarchopoulos) e Vergonha (Paul Walter Hauser) integrarão juntos com a Ansiedade na mente de Riley, assim como a Nostalgia (June Squibb) que aparecerá também."
}

PUT br_text/_doc/3
{
  "titulo": "meu malvado favorito 4",
  "message": "Nesta sequência, o vilão mais amado do planeta, que virou agente da Liga Antivilões, retorna para mais uma aventura em Meu Malvado Favorito 4. Agora, Gru (Leandro Hassum), Lucy (Maria Clara Gueiros), Margo (Bruna Laynes), Edith (Ana Elena Bittencourt) e Agnes (Pamella Rodrigues) dão as boas-vindas a um novo membro da família: Gru Jr., que pretende atormentar seu pai. Enquanto se adapta com o pequeno, Gru enfrenta um novo inimigo, Maxime Le Mal (Jorge Lucas) que acaba de fugir da prisão e agora ameaça a segurança de todos, forçando sua namorada mulher-fatal Valentina (Angélica Borges) e a família a fugir do perigo. Em outra cidade, as meninas tentam se adaptar ao novo colégio e Valentina incentiva Gru a tentar viver uma vida mais simples, longe das aventuras perigosas que fez durante quase toda a vida. Neste meio tempo, eles também conhecem Poppy (Lorena Queiroz), uma surpreendente aspirante à vilã e os minions dão o toque que faltava para essa nova fase."
}

PUT br_text/_doc/4
{
  "titulo": "o sexto sentido",
  "message": "O psicólogo infantil Malcolm Crowe (Bruce Willis) abraça com dedicação o caso de Cole Sear (Haley Joel Osment). O garoto, de 8 anos, tem dificuldades de entrosamento no colégio e vive paralisado de medo. Malcolm, por sua vez, busca se recuperar de um trauma sofrido anos antes, quando um de seus pacientes se suicidou na sua frente."
}

```

# Update the documents with embedding data
```sh
POST br_text/_update_by_query?pipeline=ml-35-small
```
