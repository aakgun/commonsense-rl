## IQA Cleanup Games

This directory contains a set of 45 games created based on the dataset available in the folder ```iqa_dataset``` (under ```game_generation/iqa_cleanup``` directory).
This set of games has been generated using the script:
```bash
$ ./make_cleanup_games.sh
```

#### Difficulty level
The games are grouped into three difficulty levels as follows.

* **Easy level**: games in the ```easy``` directory have only *1 room and 1 object* that need to be placed in
the appropriate location.
* **Medium level**: games with a ```medium``` difficulty level have _1_ room and _3_ objects shuffled across the room. Given the high number of objects,
    in this case we expect that the agent will need to rely more on the commonsense knowledge graph.
* **Hard level**: games with a ```hard``` difficulty level have _1_ or _2_ rooms with 6 or 7 objects shuffled across the rooms. In this case, we expect that the agent needs to leverage other knowedge sources such as historical state description etc efficiently in addition to the commonsense knowledge.
    
More complex games can be generated with our ```game_generation``` folder.
#### Training and test set
For each difficulty level, _15_ cleanup games have been generated and separated into 3 directories/splits.
* ```train```: this directory contains 5 games generated using a subset that accounts
for two thirds of the whole list of entities defined in ```iqa_dataset```.
* ```test```: this directory contains 5 test games that involve only the remaining entities that have not been included
in the games used for training purposes. This set of games is meant to measure the compositional generalization
capabilities of the agent with respect to the commonsense knowledge graph.
* ```valid```: this directory further provides 5 test games that share the same set of entities that has been
used to generate the training games.

#### Game structure
Some details about the structure of each game can be directly interpreted from the name of the file.
As an example, the game

```
./medium/train/tw-iqa-cleanup-objects5-take4-rooms4-train-pkOMukdBcYEGuqx0.ulx
```

has 5 objects shuffled in 4 rooms. One of these objects is already placed in the inventory at the beginning of the game.
Therefore the agent only has to take 4 out of the 5 objects in the environment. The game has been generated using the
subset of the entities reserved for training purposes.

More details about the structure of each game can be accessed by inspecting its metadata.
For instance, the maximum score and the complete list of entities in each game can be retrieved as follows:

```python
from textworld import Game
game = Game.load("<file-name>.json")
max_score = game.metadata["max_score"]
entities = game.metadata["entities"]
```

In addition to the game files, we have also included the commonsense subgraph extracted from ConceptNet:
* _via manual/human annotation_: contains manually annotated triplets relevant to the goal of a game. This file is generated for each game by a human expert. 
  We use```conceptnet_manual_subgraph-*.tsv``` as the name of the file for each game under ```manual_subgraph``` folders.
* _via _automated extraction_: contains automatically extracted subgraph generated using the entities and their 2-hop neighbors used in that split (train, test, valid) for each difficulty level.
 The file ```conceptnet_subgraph.txt``` can be found under each split for each difficulty level. These files are generated to avoid loading the entire ConceptNet graph.