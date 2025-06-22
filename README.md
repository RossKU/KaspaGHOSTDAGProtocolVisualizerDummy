
# Kaspa GHOSTDAG Protocol Visualizer



Welcome to the Kaspa GHOSTDAG Protocol Visualizer, a real-time, interactive web application designed to demystify the core principles of Kaspa's revolutionary consensus mechanism. This tool fetches live data from the Kaspa network and visualizes how the GHOSTDAG protocol orders a high-throughput stream of parallel blocks into a single, consistent ledger.

Unlike traditional blockchain visualizers, this project focuses on providing a **high-fidelity simulation** of the GHOSTDAG protocol's internal logic, making it an excellent educational resource for developers, researchers, and anyone interested in understanding what makes Kaspa unique.

## ✨ Live Demo

Experience the visualizer in action right in your browser. No installation required.

**[➡️ Launch the Visualizer](https://htmlpreview.github.io/?https://github.com/RossKU/KaspaGHOSTDAGProtocolVisualizerDummy/blob/main/Live%20v0.2.2.html)** 

## Key Features

- **Real-Time Visualization**: Connects directly to the Kaspa API to display the blockDAG as it grows.
- **Accurate GHOSTDAG Coloring**: Implements the core protocol rules to correctly color blocks `blue` (part of the honest chain) and `red` (competing blocks outside the K-cluster).
- **Virtual Chain Highlighting**: The most heavily-weighted chain, known as the "virtual chain," is prominently highlighted, showing the final, ordered sequence of blocks.
- **Interactive Controls**: Pause the simulation, reset the view, zoom in/out, and switch between display modes to inspect the DAG's structure at your own pace.
- **Multiple Processing Modes**: Analyze how the DAG is processed by switching between different block sorting orders (Receive Order, Timestamp Order, etc.).
- **Detailed Block Information**: Click on any block to open a modal with its detailed raw data, including hash, parent hashes, blueScore, and more.
- **Responsive Design**: Optimized for both desktop and mobile viewing.

## The Core: A High-Fidelity Protocol Implementation

The primary goal of this project is not just to draw a graph, but to accurately simulate the decision-making process of a real Kaspa node. We have paid special attention to implementing the nuanced details of the GHOSTDAG protocol.

### 1. The GHOSTDAG Algorithm
In a traditional blockchain, blocks are created in a single chain. If two blocks are mined at the same time, one is typically orphaned and discarded.

Kaspa's GHOSTDAG is different. It's a Directed Acyclic Graph (DAG) where all blocks are incorporated into the ledger. Instead of being discarded, parallel blocks are ordered based on a sophisticated set of rules. The visualizer shows this process in real-time:
- **`Blue Blocks`**: These are considered part of the "main chain" or its most closely related side-chains. They contribute to the final ordering.
- **`Red Blocks`**: These blocks are also valid but were created in parallel with too many other blocks (i.e., they have a large "anticone"). While they are not included in the primary ordering of the chain, their transactions are still valid and referenced by future blue blocks.

### 2. Accurate Parent Selection
A crucial step in GHOSTDAG is for each new block to select its "best" parent to build upon.
- **Our Implementation**: This visualizer correctly selects a block's main parent based on the **highest `blueWork`** (the cumulative proof-of-work of its blue-colored past). In the case of a tie, it uses the block hashes as a deterministic tie-breaker.
- **Why it Matters**: This is the exact mechanism that secures the network and ensures all nodes converge on the same chain history. A simpler model might incorrectly choose a parent, leading to an inaccurate representation of the virtual chain.

### 3. Precise Anticone Calculation
The coloring of blocks (blue vs. red) depends on the size of their "anticone"—the set of parallel, non-causally related blocks.
- **Our Implementation**: We have implemented a correct anticone calculation that considers both a block's **past set** (all blocks it references) and its **future set** (all blocks that reference it).
- **Why it Matters**: A naive implementation might only check the past, leading to an oversized anticone and the incorrect classification of valid blue blocks as red. Our precise calculation ensures the visual coloring accurately reflects the protocol's state.

### 4. Visual Finality Mechanism
To enhance performance and represent the concept of consensus finality, the visualizer implements a "visual finality" mechanism.
- **Our Implementation**: Once a block reaches a certain depth from the virtual chain (`VISUALIZER_FINALITY_DEPTH = 15`), its color is considered final, and it's excluded from future, computationally-expensive recalculations.
- **Why it Matters**: In a real network, the deeper a block is buried in the DAG, the more secure and immutable it becomes. This logic not only makes the visualizer significantly more performant but also mirrors this core concept of "finality," showing how the consensus solidifies over time.

## How to Use the Visualizer

The controls at the bottom right allow you to interact with the simulation:

- **🌙 / ☀️**: Toggle between Dark and Light mode.
- **Rx / Ts / ...**: Cycle through different block `Processing Modes`.
- **Pause / Resume**: Pause or resume the animation and data fetching.
- **Reset**: Clear the current view and start over.
- **- / +**: Zoom out or zoom in on the DAG.

## Processing Modes Explained

You can change how incoming blocks are processed, which is useful for analysis:
- **Rx (Receive Order)**: The default. Blocks are processed in the order they are received from the API. This is the most "natural" view.
- **Ts (Timestamp Order)**: Blocks are sorted and processed by their timestamp. This can cause large backlogs if blocks arrive out of order, and is a great way to test the "fast-forward" feature.
- **DAA / Bs / Bw**: Blocks are sorted by DAA Score, Blue Score, or Blue Work, respectively. These are useful for advanced analysis of the protocol's metrics.

## Technical Details

- Built with pure, dependency-free **JavaScript, HTML, and CSS**.
- Uses the **HTML5 Canvas API** for high-performance rendering of the DAG's connection lines.
- Block elements are lightweight DOM objects, animated with CSS transforms.
- Features a smart **Fast-Forward** mechanism to quickly process large block caches without freezing the UI.
- Implements a **10-second rule** for resuming from pause to prevent unintentional, large block downloads.

## Feedback and Contributions

This project is a work in progress, and we welcome your feedback and contributions. If you find a bug, have a suggestion, or want to contribute to the code, please feel free to:
- **Open an issue** on GitHub.
- **Submit a pull request**.

## Acknowledgments

A special thank you to the **Kaspa developers and the vibrant community** for creating this groundbreaking technology and for providing the public API that makes this project possible.

## License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.
