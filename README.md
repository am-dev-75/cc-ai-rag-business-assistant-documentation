# cc-ai-rag-business-assistant-documentation

Documentation about Cheshire Cat AI-based Assistant conceived to deal with business knowledge base.

# Knowledge base: [https://wiki.dave.eu](https://wiki.dave.eu)

## Accuracy benchmarking

### Introduction

This chapter lists some test results used to compare different configurations in terms of accuracy. The answer to a simple, specific question has been used to "benchmark" them. The testbed used to build the knowledge base is this website: [https://wiki.dave.eu](https://wiki.dave.eu).

The benchmark question is easy at first sight, but it a little bit subtle, instead. AURA SoM of one of the products described in the website. Specifically, it is a so-called "system-on-module" (SoM), i.e. an electronic board conceived to build industrial platforms. Its power consumption measurements are detailed in this page [https://wiki.dave.eu/index.php/AURA_SOM/AURA_Hardware/Electrical_Thermal_and_Mechanical_Features/Operational_characteristics](https://wiki.dave.eu/index.php/AURA_SOM/AURA_Hardware/Electrical_Thermal_and_Mechanical_Features/Operational_characteristics). As stated in this page, it is not very useful to provide a theoretical maximum power consuption, even though it would be possible calculate it. Instead, several real use cases are reported, including reasonable worst case scenarios. Take into account, however, that power measurements are either tabulated or visualized in a chart. A such, the ideal process to determine the answer should look like this:

1. Extract the maximum power consumptions (P1) from the tables
2. Visually, extract the maximum power consumptions (P2) from the charts
3. Answer max(P1, P2) and stress the fact that
   * This is not a theoretical maximum consumption
   * Even greater consumption could be experiencd in real-word configurations.

To perform step #2, of course it is necessary to work with a multimodal embedding model capable of handling images as well. Please note that not all the embedders have been testing are multimodal.

### Results


| Testbed setup<br />#   | CC AI version       | Web scraper / crawler (1) | Language model | Embedder                               | Declarative memory points | Benchmark<br />"What is the maximum power consumption of AURA SoM?"                                                                                                                   |
| ------------------------ | --------------------- | --------------------------- | ---------------- | ---------------------------------------- | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0<br />(golden answer) | n/a<br />(DAVE Bot) | n/a                       | n/a            | n/a                                    | n/a                       | The maximum power consumption of the AURA SOM is up to 2W. Let us know how to contact you!                                                                                            |
| 1                      | 1.9.2               | web-scraper-crawler-00    | Llama3.2:3b    | BAAI/bge-base-en<br />512<br />passage | 8643                      | *grin* Ahah, my curious friend! According to the text, the maximum power consumption of the AURA SOM (System-on-Module) is 3.68 W. *disappears and reappears with a mischievous grin* |
|                        |                     |                           |                |                                        |                           |                                                                                                                                                                                       |

(1)

* web-scraper-crawler-00: [https://github.com/am-dev-75/web-scraper-crawler-00](https://github.com/am-dev-75/web-scraper-crawler-00https:/)

#### Comments

##### Testbed setup #0 (golden answer)

This testbed setup refers to the DAVE Bot that is integrated in the website. The answer is not correct. Assumiming that the DAVE Bot is not able to process images, the maximum power consumption reported [here](https://wiki.dave.eu/index.php/AURA_SOM/AURA_Hardware/Electrical_Thermal_and_Mechanical_Features/Operational_characteristics) is ~2.8W.

##### Testbed setup #1

The answer is a clear example of erroneous "reasoning". By exploring the documents used by CC to answer, you can see that it used (in descending order of ranking):

* [https://wiki.dave.eu/index.php/AURA_SOM/AURA_Hardware/Electrical_Thermal_and_Mechanical_Features/Operational_characteristics](https://wiki.dave.eu/index.php/AURA_SOM/AURA_Hardware/Electrical_Thermal_and_Mechanical_Features/Operational_characteristicshttps:/)
* [https://wiki.dave.eu/index.php/Thermal_tests_and_measurements_(SBCX)](https://wiki.dave.eu/index.php/Thermal_tests_and_measurements_(SBCX)https:/)

Although the first link is correct to be considered the most relevant, CC got confused and used a value (3.8W) retrieved from the second link, which has nothing to do with the queried product.
