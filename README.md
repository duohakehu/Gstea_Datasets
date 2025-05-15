# Dataset Documentation

## 📂 File Structure

### 1. `networking_num_NN+UEnode_data.xlsx`

- **File Naming Convention**:
  - `num`: Network type;
  - `NN`: Number of network nodes;
  - `UEnode`: Number of unmanned vehicles (users);

- **Dataset Content**:
  1. **First column**: Availability result;
  2. **Columns 2 to second-to-last**: Node features, with every 4 columns corresponding to one node's attributes:
     - Columns 2–5: Features of node 1;
     - Columns 6–9: Features of node 2;
     - And so on...
     - Each node feature set includes:
       - Mean Time Between Failure (MTBF)
       - Mean Time To Repair (MTTR)
       - Failure Detection Rate
       - Service Migration Probability
  3. **Last column**: Event sequence for the current sample, where each event causes a change in network resources.

- **Note**:
  - The resource changes corresponding to the event sequence are recorded in `networking_num_NN+UEnode__data__ek.json`.

---

### 2. `networking_num_NN+UEnode_ek.json`

- **Purpose**:
  - Records the resource changes of each node during every event for all samples.
  - Each change corresponds one-to-one with the event sequence in `networking_num_NN+UEnode_data.xlsx`.

---

### 3. `networking_1_28+8__data__topology.json`

- **Purpose**:
  - Stores the network topology for each sample.
  - Each sample's topology is stored as a list:
    - List length = initial topology + number of user behavior events;
    - A new topology is read whenever a user behavior pattern changes;
    - Fault and recovery events do not require reloading topology.

- **Topology Handling**:
  - **Fault events**:
    - Remove the faulty node and its edges to neighbor nodes from the current topology;
  - **Topology Encoding**:
    - Each topology is stored in binary format within the JSON file;
    - To convert to an adjacency matrix, use the following Python code:

```python
import base64
import pickle

binary_adj = base64.b64decode(base64_str)
adj = pickle.loads(binary_adj)
```

---