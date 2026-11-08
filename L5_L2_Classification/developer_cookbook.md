# Developer Cookbook — L_OPENAIRINTERFACE
**Stack:** Python 3.11, OAI (C++ build + Python management API), PAX 27B, AIOSS_FORMAT
**Domain:** OpenAirInterface: open-source 4G/5G RAN for Anticloud private network deployments

## Monitor OAI gNB
```python
from l_openairinterface import OAIManager

manager = OAIManager(
    gnb_config="./oai_gnb.conf",
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./oai.aioss"
)

status = manager.get_status()
print(f"Active UEs: {status.n_ues}")
print(f"Cell throughput: DL={status.dl_mbps:.1f}, UL={status.ul_mbps:.1f}")
print(f"PAX optimization: {manager.pax_optimize_scheduling()}")
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```
