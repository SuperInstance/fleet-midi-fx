# fleet-midi-fx

_Effects routing — wet, dry, or somewhere in between._

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for fx**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → fx (port 2172)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-fx",
  "port": 2172,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | wet (processed) | balanced | dry (unprocessed) |

## Educational Supplement

Effects processors modify the sound. The fundamental decision is the wet/dry mix —
how much processed signal vs. how much original signal.

### Wet vs. Dry
- **Dry (-1)**: 100% original signal. Unprocessed, direct, present.
- **Balanced (0)**: 50/50 blend. Natural but enhanced.
- **Wet (+1)**: 100% processed signal. Immersive, textural, ambient.

### Effect Types
- **Reverb**: Simulates room acoustics. Creates depth and space.
- **Delay**: Echo effects. Creates rhythm and movement.
- **Chorus/Flanger**: Comb filtering. Creates thickness and shimmer.
- **Distortion**: Harmonic saturation. Creates edge and aggression.

## Fleet Integration

- **Port**: 2172
- **Roles**: spatial, cc
- **Conductor ID**: `fx`
- **Protocol**: HTTP POST to `/2172/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2172
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
