# Deploy Guide — L_OPENAIRINTERFACE
**Tier:** TIER_8_RF_MESH_DEFENSE | **Stack:** Python 3.11, OAI (C++ build + Python management API), PAX 27B, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, OAI (separate C++ build, Ubuntu 22.04). USRP B210 or srsRAN simulated gNB. 32GB RAM.

## Environment
Ubuntu 22.04. OAI C++ build. USRP hardware or simulated gNB. 32GB RAM.

## AIOSS Integration
```bash
aioss init --module L_OPENAIRINTERFACE --output ./l_openairinterface.aioss
aioss append --chain ./l_openairinterface.aioss --payload ./output.bin --module L_OPENAIRINTERFACE
aioss verify --chain ./l_openairinterface.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="L_OPENAIRINTERFACE",
    aioss_chain="./L_OPENAIRINTERFACE.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./L_OPENAIRINTERFACE.aioss --verbose
python -m L_OPENAIRINTERFACE.tests.smoke
```
