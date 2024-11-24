# 🎥 YouTube Automático - IA's Gratuitas

#### Vídeo: https://youtu.be/ZoJc8OJjS_s

## 📝 Prompts

### TABELA HISTÓRIAS - Fonte

"Em tempos modernos, a solidão se tornou o maior desafio para os homens. Estudos mostram que mais de 60% dos jovens relatam dificuldade em formar conexões reais, enquanto enfrentam pressões esmagadoras da sociedade. Redes sociais prometem conexão, mas entregam comparação e isolamento, criando padrões irreais que corroem a autoestima.

A cultura atual, que enfatiza sucesso individual, muitas vezes desvaloriza vulnerabilidade e empatia. Homens que buscam alcançar padrões inalcançáveis frequentemente acabam emocionalmente exaustos e desconectados, presos em um ciclo de insatisfação e insegurança.

Especialistas sugerem que romper este ciclo é essencial, priorizando autenticidade e relacionamentos genuínos. Pequenas ações, como abrir espaço para conversas sinceras e redefinir o que significa 'sucesso', podem ser o primeiro passo para reconstruir comunidades e ajudar os homens a se reconectarem consigo mesmos e com os outros."

### TABELA TIPOS DE HISTÓRIA – Prompt

Crie uma história detalhada e envolvente no estilo Red Pill.  
O tema da história é: O preço da validação feminina: por que muitos homens perdem sua identidade?  

A história deve seguir esta estrutura:  
1. Introdução impactante que prenda a atenção do público.  
2. Contexto que explique o problema principal.  
3. Desenvolvimento em 80 a 90 seções, cada uma explorando um aspecto diferente do tema. Sem numeração e sem marcaçao do tipo 1. 2., etc
4. Conclusão que resuma os pontos principais e inclua uma mensagem provocativa ou inspiradora.  

Use uma linguagem reflexiva, mas acessível, e adicione exemplos ou dados relevantes para tornar a história mais envolvente.  
O texto deve ter entre 10.000 e 15.000 palavras para um vídeo de 12 a 15 minutos.

### TABELA TIPOS DE HISTÓRIA – Cenas Prompts JSON

Eu tenho uma história para a qual preciso gerar prompts para criação de imagens com IA. A história será dividida em 25 cenas, e cada cena deve ser ilustrada com 3 imagens detalhadas no estilo de quadrinhos, totalizando 75 imagens. Cada imagem deve capturar uma parte da narrativa, representando cerca de 10 segundos no vídeo.

Por favor, formate os resultados como um objeto JSON contendo um array chamado "pairs". Cada objeto dentro do array deve conter:

"scene": O número da cena (de 1 a 25).
"original": A sentença ou parágrafo da história que corresponde à cena.
"aiImagePrompts": Um array de três strings, onde cada string é um prompt detalhado para criar uma imagem da cena no estilo de quadrinhos.
Diretrizes para os Prompts:

Declaração de Estilo de Quadrinhos: Comece cada prompt com "Crie uma ilustração dinâmica e vívida de quadrinhos de...".
Descrição dos Personagens e Ações: Descreva os personagens, suas ações e interações específicas na cena.
Cenário/Fundo: Inclua detalhes sobre o ambiente ou fundo para reforçar o contexto da cena.
Humor e Tema: Reflita o tom emocional e os temas da história em cada ilustração.
Elementos Visuais: Destaque aspectos visuais específicos, como expressões, tecnologia, interações sociais ou símbolos relevantes.
A ideia é criar prompts criativos e envolventes que capturem a essência de cada parte da história e proporcionem ilustrações detalhadas e ricas em narrativa.

Aqui está a história que quero usar:

## TABELA TIPOS DE HISTÓRIA - Model ID

aa77f04e-3eec-4034-9c07-d0f619684628

## URLs

Gerar imagem Leonardo AI - https://cloud.leonardo.ai/api/rest/v1/generations
Buscar imagem Leonardo AI - https://cloud.leonardo.ai/api/rest/v1/generations/[INSERIR_GENERATION_ID]
Gerar vídeo JSON2Video - https://api.json2video.com/v2/movies
Gerar áudio Elevenlabs - https://api.elevenlabs.io/v1/text-to-speech/[INSERIR_VOICE_ID]

## Fórmulas

### Tabela Histórias

```bash
lookup('Tipos de História', 'Prompt')

lookup('Tipos de História', 'Largura Output')

lookup('Tipos de História', 'Altura Output')

lookup('Tipos de História', 'Largura Imagem')

lookup('Tipos de História', 'Altura Imagem')

lookup('Tipos de História', 'Cenas Prompts JSON')
```

## 👨‍💻 Códigos para geração da História e Cenas

### Preparar prompt node

```bash
const data = items[0].json;

const fonte = data["Fonte"];
const promptTipoHistoria = data["Prompt"][0].value;

return [
  {
    json: {
      fonte: fonte,
      prompt: promptTipoHistoria
    }
  }
];
```

### Organizar dados node

```bash
const openaiData = items[0].json.message.content;
const baserowData = items[1].json;

const combineContentValues = (data) => {
  return Object.values(data).flatMap(value => {
    if (Array.isArray(value)) {
      return value.join(", ");
    } else if (typeof value === 'object' && value !== null) {
      return [];
    } else {
      return value;
    }
  }).join("\n");
};

const combinedOutput = combineContentValues(openaiData);

return [
  {
    json: {
      id: baserowData.id,
      combinedOutput: combinedOutput.trim()
    }
  }
];
```

### Preparar prompt node 2

```bash
const mergedData = items;

const data1 = mergedData[0].json;
const data2 = mergedData.length > 1 ? mergedData[1].json : {};

let historiaCompleta = "";

for (const item of mergedData) {
  if (item.json["História"]) {
    historiaCompleta = item.json["História"];
    break;
  }
}

const cenasPromptsJsonString = data1["Cenas Prompts JSON"] || "";
const historiaArray = data1["Histórias"] || [];

const cenasPromptsJson = cenasPromptsJsonString.split("\n").join("\n");
const historia = historiaArray.map(item => item.value).join("\n");

const prompt = `
  ${cenasPromptsJson}

  ${historiaCompleta}
`;

return [
  {
    json: {
      prompt: prompt.trim()
    }
  }
];
```

### Transformar array (\*Converter array para lista)

```bash
const pairs = items[0].json.message.content.pairs;
const result = [];

for (const pair of pairs) {
  for (const prompt of pair.aiImagePrompts) {
    result.push({ json: { aiImagePrompt: prompt, original: pair.original } });
  }
}

return result;
```

## 👨‍💻 Códigos para geração de Imagem e Vídeo

### Transformar array (\*Converter array para lista 2)

```bash
return items
  .map(item => {
    return {
      json: {
        "prompt": item.json['AI Image Prompts']
      }
    };
  });
```

### Extrair dimensão da imagem + model ID

```bash
const inputData = items[0].json;

const outputData = {
  width: inputData["Largura Imagem"],
  height: inputData["Altura Imagem"],
  modelId: inputData["Model ID"],
  prompt: inputData["Prompt"]
};

return [{ json: outputData }];
```

### Mesclar outputs por delay do merge

```bash
const inputData = items[0].json;

return items.slice(1).map(item => {
  return {
    json: {
      prompt: inputData.prompt,
      width: parseInt(inputData.width),
      height: parseInt(inputData.height),
      modelId: inputData.modelId
    }
  };
});
```

### Extrair AI Image Prompts

```bash
const items = $input.all();

const seenIds = new Set();
const uniquePrompts = [];

for (const item of items) {
    const data = item.json;
    if (!seenIds.has(data.id)) {
        seenIds.add(data.id);
        uniquePrompts.push({
            id: data.id,
            'AI Image Prompts': data['AI Image Prompts']
        });
    }
}

return uniquePrompts.map(prompt => ({ json: prompt }));
```

### Mesclar outputs por delay do Merge3

```bash
const items = $input.all();

const prompts = items.filter(item => item.json.id !== undefined);
const properties = items.filter(item => item.json.width !== undefined);

const mergedResults = prompts.map((prompt, index) => {
    const property = properties[index % properties.length];
    return {
        json: {
            id: prompt.json.id,
            "AI Image Prompts": prompt.json["AI Image Prompts"],
            image_size: {
                width: property.json.width,
                height: property.json.height
            },
            modelId: property.json.modelId
        }
    };
});

return mergedResults;

```

### Filtrar ID linha + ID Geração

```bash
const items = $input.all();

let baserowItems = [];
let generationItems = [];

for (let i = 0; i < items.length; i++) {
    let item = items[i].json;

    if (item.sdGenerationJob) {
        generationItems.push(item);
    } else {
        baserowItems.push(item);
    }
}

let output = [];

for (let i = 0; i < baserowItems.length; i++) {
    let baserowItem = baserowItems[i];

    if (generationItems[i] && generationItems[i].sdGenerationJob) {
        let generationId = generationItems[i].sdGenerationJob.generationId;
        let id = baserowItem.id;

        output.push({ id, generationId });
    }
}

return output;
```

### Extrair ID Geração

```bash
const inputData = items.map(item => item.json);

const uniqueIDs = new Set();

inputData.forEach(item => {
  uniqueIDs.add(item["ID Geração"]);
});

const uniqueIDArray = Array.from(uniqueIDs);

return uniqueIDArray.map(id => ({ "ID Geração": id }));
```

### Identificar URL por ID da linha

```bash
const results = [];
const baserowData = [];
const imageData = [];

for (const item of items) {
  if (item.json.generations_by_pk) {
    imageData.push(item.json);
  } else {
    baserowData.push(item.json);
  }
}

for (const imageItem of imageData) {
  const generation = imageItem.generations_by_pk;
  if (generation && generation.generated_images.length > 0) {
    const imageUrl = generation.generated_images[0].url;
    const generationId = generation.id;

    const originalItem = baserowData.find(row => row['ID Geração'] === generationId);

    if (originalItem) {
      const rowId = originalItem.id;

      results.push({
        json: {
          row_id: rowId,
          image_url: imageUrl
        }
      });
    }
  }
}

return results;
```

### Extrair ID linha + URL imagem

```bash
const inputData = $input.all();

const uniqueItems = [];
const seenIds = new Set();

for (const item of inputData) {
  const id = item.json.id;
  const imagem = item.json.Imagem;

  if (!seenIds.has(id)) {
    seenIds.add(id);
    uniqueItems.push({ id, imagem });
  }
}


return uniqueItems;
```

### Extrair altura e largura output

```bash
const inputData = $input.all();

if (inputData.length > 0) {
  const firstItem = inputData[0].json;

  const larguraOutput = firstItem["Largura Output"] && firstItem["Largura Output"].length > 0
    ? firstItem["Largura Output"][0].value
    : null;

  const alturaOutput = firstItem["Altura Output"] && firstItem["Altura Output"].length > 0
    ? firstItem["Altura Output"][0].value
    : null;

  return [
    {
      larguraOutput,
      alturaOutput
    }
  ];
} else {
  return [
    {
      larguraOutput: null,
      alturaOutput: null
    }
  ];
}
```

### Preparar input para JSON2Video

```bash
let elementId = 1;
const processedLinks = new Set();
let elements = [];

let validLarguraOutput = null;
let validAlturaOutput = null;

items.forEach(item => {
  const larguraOutput = item.json["larguraOutput"];
  const alturaOutput = item.json["alturaOutput"];

  if (larguraOutput && alturaOutput) {
    validLarguraOutput = parseInt(larguraOutput, 10);
    validAlturaOutput = parseInt(alturaOutput, 10);
  }
});

items.forEach(item => {
  const id = item.json["id"];
  const imagem = item.json["imagem"];

  if (id && imagem) {
    let larguraOutput = validLarguraOutput;
    let alturaOutput = validAlturaOutput;

    if (!processedLinks.has(imagem) && larguraOutput && alturaOutput) {
      processedLinks.add(imagem);
      elements.push({
        id: elementId++,
        type: "image",
        src: imagem,
        duration: 2,
        zoom: 2,
        width: larguraOutput,
        height: alturaOutput,
        position: "center-center"
      });
    }
  }
});

let videos = [];
for (let i = 0; i < elements.length; i += 3) {
  const scenes = [];
  for (let j = 0; j < 3 && i + j < elements.length; j++) {
    scenes.push({
      comment: `Scene #${Math.floor(i / 3) + 1}`,
      elements: [elements[i + j]]
    });
  }

  if (scenes.length > 0) {
    const videoJson = {
      resolution: "full-hd",
      quality: "high",
      scenes: scenes
    };
    videos.push({ json: videoJson });
  }
}

return videos;
```

## 👨‍💻 Códigos para geração de Áudio

### Extrair conteúdo da coluna Cena

```bash
const uniqueScenes = {};
const data = items;

data.forEach(item => {
  const scene = item.json;
  if (!uniqueScenes[scene.Cenas]) {
    uniqueScenes[scene.Cenas] = scene.Cenas;
  }
});

const uniqueScenesArray = Object.values(uniqueScenes);

return uniqueScenesArray.map(scene => ({ json: { Cena: scene } }));
```

### Extrair ID coluna + URL áudio

```bash
const items = $input.all();

const videoItems = items.filter(item => item.json.hasOwnProperty('ID Vídeo'));
const audioItem = items.find(item => item.json.mimeType === 'audio/mpeg');

let videoItemToUpdate = null;

for (let video of videoItems) {
  if (!video.json.Áudio) {
    if (videoItemToUpdate === null || video.json.id < videoItemToUpdate.json.id) {
      videoItemToUpdate = video;
    }
  }
}

let result = {};

if (videoItemToUpdate) {
  videoItemToUpdate.json.Áudio = audioItem.json.webContentLink;
  result = {
    id: videoItemToUpdate.json.id,
    Áudio: videoItemToUpdate.json.Áudio
  };
}

return [result];
```

## 👨‍💻 Códigos para Áudio + Vídeo

### Extrair id + link Vídeo e Áudio

```bash
const inputData = items;

let filteredData = inputData.map(item => ({
  id: item.json.id,
  video: item.json.Vídeo,
  audio: item.json.Áudio
}));

let uniqueData = [];
let ids = new Set();

filteredData.forEach(item => {
  if (!ids.has(item.id)) {
    uniqueData.push(item);
    ids.add(item.id);
  }
});

return uniqueData.map(data => ({ json: data }));
```

### Extrair dimensão da imagem 2

```bash
const inputData = items[0].json;

const outputData = {
  width: inputData["Largura Output"],
  height: inputData["Altura Output"]
};

return [{ json: outputData }];
```

### Preparar input para JSON2Video 2

```bash
const items = $input.all();

const { width, height } = items[0].json;

const videoRequests = [];

items.slice(1).forEach((item, index) => {
  const scenes = [
    {
      comment: `Scene #${index + 1}`,
      elements: [
        {
          type: 'video',
          src: item.json.video,
          duration: -2
        }
      ],
    }
  ];

  const audioElements = [
    {
      type: 'audio',
      src: item.json.audio,
      duration: -1
    }
  ];

  const requestPayload = {
    id: 'Vídeo + Áudio',
    fps: 25,
    cache: false,
    draft: false,
    width: parseInt(width, 10),
    height: parseInt(height, 10),
    scenes: scenes,
    quality: 'high',
    elements: audioElements,
    settings: {},
    resolution: 'custom'
  };

  videoRequests.push({ json: requestPayload });
});

return videoRequests;
```

### Extrair ID linha

```bash
const items = $input.all();

const uniqueIds = new Set();

items.forEach((item) => {
  if (item.json && typeof item.json === 'object' && item.json.id) {
    uniqueIds.add(item.json.id);
  }
});

const uniqueIdsArray = Array.from(uniqueIds);

return uniqueIdsArray.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay do merge 2

```bash
function mergeOutputs(inputs) {
    let mergedOutput = [];
    let ids = inputs.filter(input => input.json.id !== undefined);
    let movies = inputs.filter(input => input.json.movie !== undefined);

    ids.forEach((id, index) => {
        if (index < movies.length) {
            mergedOutput.push({
                ...id.json,
                ...movies[index].json
            });
        } else {
            mergedOutput.push(id.json);
        }
    });

    movies.slice(ids.length).forEach(movie => {
        mergedOutput.push(movie.json);
    });

    return mergedOutput;
}

let inputs = $input.all();

if (!inputs || inputs.length === 0) {
    throw new Error("No inputs received.");
}

let output = mergeOutputs(inputs);

return output;
```

## 👨‍💻 Códigos para Vídeo + Legenda

### Extrair ID e Áudio + Vídeo

```bash
function processInput(items) {
  let seenIds = new Set();

  let result = [];

  if (Array.isArray(items)) {
    items.forEach(item => {
      let currentItem = item.json || {};

      if (currentItem.id && currentItem["Vídeo + Áudio"]) {
        if (!seenIds.has(currentItem.id)) {
          seenIds.add(currentItem.id);
          result.push({ id: currentItem.id, "Vídeo + Áudio": currentItem["Vídeo + Áudio"] });
        }
      }
    });
  }
  return result;
}

let items = $input.all();

let output = processInput(items);

return output;
```

### Extrair altura e largura output 2

```bash
const inputData = $input.all();

if (inputData.length > 0) {
  const firstItem = inputData[0].json;

  const larguraOutput = firstItem["Largura Output"] && firstItem["Largura Output"].length > 0
    ? firstItem["Largura Output"][0].value
    : null;

  const alturaOutput = firstItem["Altura Output"] && firstItem["Altura Output"].length > 0
    ? firstItem["Altura Output"][0].value
    : null;

  return [
    {
      larguraOutput,
      alturaOutput
    }
  ];
} else {
  return [
    {
      larguraOutput: null,
      alturaOutput: null
    }
  ];
}
```

### Preparar input para JSON2Video 3

```bash
let elementId = 1;
const processedLinks = new Set();
let elements = [];
let validLarguraOutput = null;
let validAlturaOutput = null;

items.forEach(item => {
  const larguraOutput = item.json["larguraOutput"];
  const alturaOutput = item.json["alturaOutput"];

  if (larguraOutput && alturaOutput) {
    validLarguraOutput = parseInt(larguraOutput, 10);
    validAlturaOutput = parseInt(alturaOutput, 10);
  }
});

items.forEach(item => {
  const id = item.json["id"];
  const videoUrl = item.json["Vídeo + Áudio"];

  if (id && videoUrl) {
    let larguraOutput = validLarguraOutput;
    let alturaOutput = validAlturaOutput;

    if (!processedLinks.has(videoUrl) && larguraOutput && alturaOutput) {
      processedLinks.add(videoUrl);
      elements.push({
        id: elementId++,
        type: "video",
        src: videoUrl,
        duration: -1,
        zoom: 0,
        width: larguraOutput,
        height: alturaOutput,
        position: "center-center"
      });
    }
  }
});

let scenes = [];
elements.forEach(element => {
  scenes.push({
    elements: [element]
  });
});

const outputJson = {
  id: "Vídeo Completo",
  fps: 25,
  cache: false,
  draft: false,
  width: validLarguraOutput,
  height: validAlturaOutput,
  scenes: scenes,
  quality: "high",
  elements: [
    {
      type: "subtitles",
      language: "pt-BR",
      settings: {
        "all-caps": true,
        position: "mid-bottom-center",
        "font-size": 75,
        "font-family": "Luckiest Guy",
        "outline-width": 5
      }
    }
  ],
  settings: {},
  resolution: "custom"
};

return [{ json: outputJson }];
```

### Extrair ID linha 2

```bash
const items = $input.all();

const ids = items.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay do merge 3

```bash
function mergeOutputs(inputs) {
    let mergedOutput = [];
    let ids = inputs.filter(input => input.json.id !== undefined);
    let movies = inputs.filter(input => input.json.movie !== undefined);

    ids.forEach((id, index) => {
        if (index < movies.length) {
            mergedOutput.push({
                ...id.json,
                ...movies[index].json
            });
        } else {
            mergedOutput.push(id.json);
        }
    });

    movies.slice(ids.length).forEach(movie => {
        mergedOutput.push(movie.json);
    });

    return mergedOutput;
}

let inputs = $input.all();

if (!inputs || inputs.length === 0) {
    throw new Error("No inputs received.");
}

let output = mergeOutputs(inputs);

return output;
```

---

Made with 💙 by eduardocodes 👋
