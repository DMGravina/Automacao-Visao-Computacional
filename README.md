# **Relatório de Auditoria: Automação & Visão Computacional**

## **1\. Introdução e Contexto**

Atualmente, muitas empresas ainda utilizam sistemas legados ou painéis onde as informações críticas não estão dispostas em formato de texto estruturado, mas representadas visualmente por meio de cores de status. A proposta do meu projeto é solucionar esse problema de forma automatizada. Para isso, integrei Automação para gerar uma grade de dados, e técnicas de Visão Computacional para realizar a leitura dessas informações visuais, segmentar as cores correspondentes e auditar os resultados de maneira autônoma.

## **2\. Metodologia Aplicada**

O desenvolvimento do sistema foi realizado em Python, utilizando o ambiente do Google Colab. As principais ferramentas e abordagens que utilizei foram:

* **Automação e Dados:** Empreguei a biblioteca openpyxl para criar e preencher dinamicamente uma planilha do Excel. Para gerar a matriz de cores (10x10), utilizei a biblioteca random utilizando o meu número de matrícula de quando eu ainda estava na faculdade como semente (*seed*). Esse procedimento garante que os dados gerados possuam entropia controlada e sejam totalmente reprodutíveis.  
* **Visão Computacional:** Utilizei a biblioteca OpenCV (cv2) para o processamento das imagens. Realizei a conversão da imagem da grade do espaço de cores RGB para HSV (Matiz, Saturação e Valor). Essa técnica me permitiu definir limites no espectro de cores e criar máscaras de segmentação para isolar as cores Verde (Sucesso), Amarelo (Atenção) e Vermelho (Crítico).  
* **Visualização e Tabulação:** Apliquei o matplotlib para exibir as máscaras de segmentação de forma estruturada e o pandas para formatar a tabela analítica na fase de cruzamento de dados.

## **3\. Resultados Obtidos**

O sistema foi capaz de interpretar a imagem da grade e aplicar as máscaras de segmentação com sucesso. O comportamento prático de cada máscara ocorreu da seguinte forma:

* **Máscara Verde:** Isolou exclusivamente as células correspondentes ao status de "Sucesso".  
* **Máscara Amarela:** Segmentou apenas os indicadores de "Atenção".  
* **Máscara Vermelha:** Identificou de forma exclusiva os alertas classificados como "Crítico".

A quantidade exata de células identificadas por categoria de cor foi exportada e salva automaticamente no arquivo mascaras\_hsv\_matricula.png. Este documento visual ilustra a imagem original justaposta às perspectivas segmentadas de cada espectro analisado.

## **4\. Auditoria de Dados**

Para validar a confiabilidade do *pipeline* de Visão Computacional, realizei uma etapa de cruzamento de dados. Os dados originais ("gabarito"), injetados na planilha via rotina de Automação, foram confrontados diretamente com a detecção realizada pelo OpenCV.

Utilizando as estruturas do Pandas, minha análise comparou as seguintes métricas:

* **Gerado (Automação):** Contagem exata extraída dos códigos hexadecimais de preenchimento presentes no arquivo Excel.  
* **Detectado (CV):** Contagem de áreas e contornos válidos reconhecidos pelo OpenCV a partir das máscaras geradas.

Calculei a diferença matemática entre as fontes e constatei que a taxa de acerto (acurácia) do sistema foi de 100%. Não registrei perdas de informação, falsos positivos ou interferências por ruído durante a leitura da imagem.

**Nota Técnica:** Cabe ressaltar que essa taxa de acerto absoluta foi viabilizada pelo uso de dados sintéticos e processamento de imagem digital direta em ambiente controlado (ruído zero), livre de compressões ou variações de iluminação. Em aplicações práticas em sistemas operacionais legados ou monitores físicos reais, variações de gama e renderização exigiriam a inclusão de etapas de pré-processamento, como suavização gaussiana ou morfologia matemática, para garantir a robustez do pipeline.

## **5\. Recomendações**

Com base nas informações extraídas e segmentadas pelo sistema, apresento as seguintes recomendações direcionadas às equipes de operações e logística:

1. **Ação Imediata (Status Vermelho):** Recomendo o direcionamento prioritário da equipe para mitigar as ocorrências nas células vermelhas, pois representam gargalos críticos no processo.  
2. **Monitoramento (Status Amarelo):** Sugiro que as operações sinalizadas em amarelo sejam colocadas em estado de alerta, demandando verificações preventivas para evitar que evoluam para o status crítico.  
3. **Operação Padrão (Status Verde):** As áreas em verde atestam um fluxo adequado. Recomendo utilizá-las como modelo de referência (*benchmark*) de eficiência para outras etapas do processo.  
4. **Próximos Passos:** Para futuras melhorias no sistema, recomendo a implementação de um modelo OCR (Reconhecimento Óptico de Caracteres). Dessa forma, além de identificar o status qualitativo pela cor, a aplicação poderá extrair dados literais, como códigos de rastreamento inscritos dentro das células logísticas.