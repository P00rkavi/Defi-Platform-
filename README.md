# Defi-Platform
      Designing a decentralized finance (DeFi) platform to facilitate peer-to-peer lending and borrowing using blockchain.

This project implements a hybrid decentralized finance (DeFi) protocol that predicts dynamic ETH-based interest rates using a 2-layer LSTM time-series model and integrates those predictions into a Solidity smart contract for deterministic yield execution.The core design principle is a strict separation between:Probabilistic off-chain machine learning inference .Deterministic on-chain financial logic.This architecture preserves smart contract predictability while enabling data-driven interest rate optimization.
2. System Architecture
The system is divided into four modular layers:
      2.1 Data & Feature Engineering Layer
            ETH OHLCV data ingestion via public market APIs
            Chronological ordering and missing value handling
            MinMax normalization (fit only on training data)
            Sliding window time-series sequence generation
            Rolling volatility, EMA spreads, and momentum features
            Synthetic regime augmentation for robustness
            
      2.2 ML Prediction Layer (PyTorch)

            2-layer stacked LSTM
            Dropout regularization
            Dense output layer (predicted interest rate)
            Mean Squared Error loss
            Gradient clipping
            Early stopping
            Model checkpointing
            
      2.3 API Layer (FastAPI)

            Async REST endpoints for model inference
            Stateless architecture
            Clear separation between ML computation and blockchain logic

      2.4 Smart Contract Layer (Solidity)

            deposit()
            withdraw()
            applyPredictedRate()
            getUserBalance()
The contract consumes a bounded predicted rate and applies deterministic yield logic.

3. Dataset & Data Integrity
Data Source: Historical ETH OHLCV market data from public crypto APIs.
Fields Used:Open,High,Low,Close,Volume

4. Model Design Decisions

Why LSTM?
Crypto markets exhibit: Temporal dependency ,Volatility clustering ,Regime persistence
While tree-based models (e.g., gradient boosting) were considered, LSTM was selected for its ability to capture sequential state transitions without manual lag feature explosion.
Trade-offs:
      Higher computational cost
      Reduced interpretability

Potential production enhancement:
      Ensemble model (LSTM + gradient boosting)
      Regime classifier with model switching

5. Determinism & Off-Chain Inference
Smart contracts must remain:
      Deterministic
      Gas-efficient
      Free of floating-point arithmetic
Running ML on-chain would violate these constraints.

This preserves blockchain integrity while enabling adaptive rate modeling.
Multi-signature administrativ
Extreme Market Events :Black swan volatility may degrade model performance.
Mitigation: Synthetic stress augmentation, Ensemble modeling roadmap













