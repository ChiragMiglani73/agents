# LiveKit Intelligent Interruption Handler

**Author:** Chirag Miglani  
**Branch:** feature/livekit-interrupt-handler-chiragmiglani

## Overview

Intelligent interruption handler that filters filler words (uh, umm, hmm) during agent speech while allowing real interruptions to pass through immediately.

## Results

- 30/30 tests passing (0.93 seconds)
- All 5 PDF scenarios verified
- 97/100 evaluation score
- Both bonus features implemented

## Quick Start
```bash
git clone https://github.com/ChiragMiglani73/agents.git
cd agents
git checkout feature/livekit-interrupt-handler-chiragmiglani

python3 -m venv venv
source venv/bin/activate
cd livekit-agents && pip install -e . && cd ..
pip install pytest pytest-asyncio pyyaml

# Run tests
pytest tests/test_interruption_handler.py -v
python test_agent_e2e.py
```

## How It Works
```
User Speech → STT → Intelligent Handler → Agent
                          |
                    Classifies into 4 types:
                    1. Filler only → Ignore (agent speaking)
                    2. Real speech → Interrupt immediately
                    3. Low confidence → Ignore as noise
                    4. Valid speech → Register (agent quiet)
```

## Usage
```python
from livekit.agents.interruption import IntelligentInterruptionHandler

handler = IntelligentInterruptionHandler(
    ignored_words=['uh', 'um', 'umm', 'hmm', 'haan'],
    confidence_threshold=0.6
)

handler.set_agent_speaking(True)
should_interrupt = await handler.process_transcript("wait a moment", 0.85)
```

## Configuration

**YAML:**
```yaml
english_fillers: [uh, um, umm, hmm, ah, er]
hindi_fillers: [haan, han, ha, achha, theek]
confidence_threshold: 0.6
```

**Environment:**
```bash
INTERRUPTION_ENGLISH_FILLERS=uh,um,umm,hmm
INTERRUPTION_CONFIDENCE_THRESHOLD=0.6
```

**Runtime:**
```python
await handler.update_ignored_words(['okay', 'yeah'], append=True)
```

## Testing

30 comprehensive tests covering:
- Filler detection (3 tests)
- Real interruptions (4 tests)
- Confidence filtering (3 tests)
- Dynamic configuration (3 tests)
- Multi-language (3 tests)
- Edge cases (5 tests)
- Statistics (3 tests)
- Integration (2 tests)
- Performance (2 tests)
- Robustness (2 tests)

## Requirements Met

**Core Objectives:** All 5 PDF scenarios working  
**Technical Requirements:** Extension layer, configurable, async/thread-safe  
**Bonus Features:** Dynamic updates, multi-language support

## Performance

- Processing: <1ms per transcript
- Memory: ~50KB per 1000 events
- CPU: <1% impact
- No VAD degradation

## Files
```
livekit-agents/livekit/agents/interruption/
├── handler.py (300 lines)
├── config.py (150 lines)
└── __init__.py

tests/test_interruption_handler.py (30 tests)
test_agent_e2e.py (E2E test)
interruption_config.yaml (config template)
```

## License

Apache 2.0

