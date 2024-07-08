# DDI
Drug-Drug Interaction

## Installation

```sh
conda create -n featurization_token python=3
conda activate featurization_token

pip install torch
pip install torch_geometric
pip install pyg_lib torch_scatter torch_sparse torch_cluster torch_spline_conv
pip install selfies, deepchem, textdistance, rdkit, tensorflow, transformers
```

## Preprocess

- -d {input_data} -> csv file
- -dn {data_name} -> anything
- -t {data_type} -> smi or sel
- -pad -> True or False
```sh
python f-token.py -d data/example.csv -dn smiles_pad -t smi -pad
python f-token.py -d data/example.csv -dn smiles -t smi
python f-token.py -d data/example.csv -dn selifes_pad -t sel -pad
python f-token.py -d data/example.csv -dn selifes -t smi
```


## Features
| Name           | Description                                      | Type    | Dimension   |
|----------------|--------------------------------------------------|---------|-------------|
| symbol         | symbol in string notation                        | one-hot | vocab size  |
| atom_type      | element in periodic table                        | one-hot | vocab size  |
| hybridization  | types of hybrid orbital shape                    | one-hot | 7           |
| acceptor_donor | whether the atom hydrogen bond donor or acceptor | one-hot | 2           |
| aromatic       | whether the atom belongs to an aromatic ring     | one-hot | 1           |
| degree         | degree (0-5) of atom                             | one-hot | 7           |
| chirality      | chirality, "R" or "S" form                       | one-hot | 2           |
| total_num_Hs   | number of hydrogen bonds                         | one-hot | 6           |
| formal_charge  | electronic charge                                | Integer | 1           |
| partial_charge | partial charge                                   | Integer | 1           |
| br_symbol      | Branch/Ring symbol position                       | one-hot | 3           |
| br_scale      | Branch/Ring length or size                       | one-hot | 1           |
| br_num_subsets      | Branch/Ring subset counts                       | one-hot | 1           |

The 'vocab size' of the dimension column in the table varies depending on the diversity of symbols included in the dataset

The br_symbol, br_scale, and br_num_subsets in the table are not features provided by the RDKit library but are instead features added by the developer through custom code

### Detailed Operational Principle 

When creating featurization tokens, features are calculated at the 'atomic symbol' level, meaning that it is not possible to extract separate features for symbols related to branches or rings, and these are not included in the data

Therefore, if a branch or ring symbol is declared, the features for these special symbols are added to the atomic symbol preceding them, reflecting their features in the data

- br_symbol: The type of branch or ring declared is represented by one-hot encoding

    [1, 0, 0] = Branch symbol declared following

    [0, 1, 0] = Ring symbol declared following

    [0, 0, 0] = No branch or ring symbol declared following

- br_scale: Indicates the length of the branch or the size of the ring (the number of included atomic symbols)

- br_num_subsets: Represents the number of nested branch or ring symbols included within the declared branch or ring (number of sub-branch/ring symbols)




## Contact (Questions/Bugs/Requests)
Please submit a GitHub issue or contact me [rudwls2717@pusan.ac.kr](rudwls2717@pusan.ac.kr)

## Acknowledgements
Thank you for our [Laboratory](https://www.k-medai.com/).

If you find this code useful, please consider citing my work.