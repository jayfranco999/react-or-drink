# REACT OR DRINK - Game Design Document

## Current Version: v1.0 (Hot Seat Edition)

---

## Core Concept
Full-body pose reaction drinking game with webcam tracking. Players take turns in the hot seat doing 5 rounds of poses while the camera judges them. Miss poses = drink. Nail all 5 = nominate someone else to drink. Pure party chaos.

**Target Vibe**: Game show energy, deadpan roast commentary, maximum embarrassment

---

## Implemented Features (v1.0)

### Controls
- Full body tracking via MediaPipe Pose (33 landmarks)
- Pose matching based on arm/hand positions relative to body
- Hold mechanic (500ms) for forgiving detection
- No buttons needed during gameplay - just your body

### Game Flow
1. **Setup**: Enter 2-8 player names
2. **Wheel Spin**: Random selection for who goes first
3. **Hot Seat**: 5 rounds of pose challenges
4. **Results**: Score determines drinking penalty
5. **Nomination**: Winner picks next player (or wheel picks for losers)
6. **Repeat**: Until everyone's been roasted
7. **Leaderboard**: Final shame rankings

### The 7 Poses

| Pose Key | Display Name | Detection Logic |
|----------|--------------|-----------------|
| stickemup | STICK 'EM UP | Both wrists above shoulders |
| tpose | ASSERT DOMINANCE | Arms wide, wrists level with shoulders |
| flex | GUN SHOW | Elbows out, wrists above elbows (flexing) |
| dab | DAB ON EM | One arm up, face in other elbow |
| shrug | NOT MY PROBLEM | Hands out at shoulder level, elbows bent |
| powerstance | POWER STANCE | Both hands on hips |
| chillin | PLAY IT COOL | Both hands below hips |

### Gameplay Twists (Rounds 3-5)

| Twist | Description | Probability |
|-------|-------------|-------------|
| **BAIT & SWITCH** | Pose changes mid-round | R3: 40%, R4: 60%, R5: 80% |
| **COPYCAT** | Do the last pose again, no hints | Easy twist |
| **OPPOSITE DAY** | Do any OTHER pose | Hard twist |
| **SPEED ROUND** | 3 poses, 1 second each | Easy twist |
| **FREEZE FRAME** | Move around, then freeze on command | Hard twist |

### Drinking Damage Chart

| Score | Verdict | Penalty |
|-------|---------|---------|
| 5/5 | IMMUNE | Nominate someone to drink |
| 3-4/5 | SAFE | Nothing (boring but alive) |
| 2/5 | ROUGH | Take 1 drink |
| 1/5 | YIKES | Take 2 drinks |
| 0/5 | PATHETIC | Take 3 drinks + wheel picks next |

### Visual Design
- Game show stage background (purple gradient, neon floor)
- Spotlight effects (magenta, cyan, white)
- Small webcam preview (top right corner)
- SVG stick figure silhouettes for pose guides
- Animated spin wheel with conic gradients
- Press Start 2P retro font
- Neon glow effects throughout

---

## Commentary System

### Design Philosophy
- Deadpan, not enthusiastic
- Roasty but smart, not cringe
- Vulgar where appropriate
- Never sycophantic or over-the-top

### Commentary Categories

**Success**:
- "acceptable."
- "the bar was low. you cleared it."
- "well shit, it actually worked."

**Fail**:
- "that was... a choice."
- "your limbs have betrayed you."
- "task failed successfully. wait, no. just failed."

**Perfect (5/5)**:
- "showoff."
- "way to make everyone else look bad."
- "main character energy. gross."

**Pathetic (0/5)**:
- "genuinely impressive how bad that was."
- "that's going in the group chat."
- "did you have a stroke or was that intentional?"

**Twist-specific**:
- Bait: "plot twist, bitch.", "surprise motherfucker."
- Copycat: "hope you were paying attention."
- Opposite: "brain vs body. fight."
- Speed: "no time to think. just vibes."
- Freeze: "DON'T. MOVE."

---

## Technical Architecture

### Stack
- Single HTML file (portable)
- MediaPipe Pose for body tracking
- CSS animations for visual effects
- Vanilla JavaScript state management

### Pose Detection
```javascript
// Example: STICK 'EM UP detection
check: (lm) => {
    const leftWrist = lm[15], rightWrist = lm[16];
    const leftShoulder = lm[11], rightShoulder = lm[12];
    return leftWrist.y < leftShoulder.y - 0.1 &&
           rightWrist.y < rightShoulder.y - 0.1;
}
```

### Key Landmark Indices
- 0: Nose
- 11, 12: Shoulders (left, right)
- 13, 14: Elbows (left, right)
- 15, 16: Wrists (left, right)
- 23, 24: Hips (left, right)

### Configuration
```javascript
const config = {
    roundsPerPlayer: 5,
    holdTime: 500,           // ms to hold pose
    baseReactionTime: 3500,  // ms per round
    minReactionTime: 2000    // minimum (speeds up)
};
```

---

## Future Iterations

### v1.1 - Party Expansion
- Sound effects (countdown, success, fail, commentary voiced)
- More poses (SUPERHERO, EGYPTIAN, ROBOT, etc.)
- Team mode (2v2 hot seat battles)
- Shame cam zoom on failures

### v1.2 - Chaos Modifiers
- Drunk vision (blur effect)
- Mirror mode (flipped webcam)
- Strobe lighting
- Double or nothing rounds

### v1.3 - Tournament Mode
- Bracket system for large groups
- Elimination rounds
- Championship final with all twists
- Loser bracket redemption

### v1.4 - Social Features
- Shareable "receipt" of shame
- Highlight reel capture
- QR code game joining
- Spectator voting

---

## Competitive Balance Notes

### Why Hold Mechanic?
- Pose detection isn't instant-perfect
- 500ms hold prevents false positives
- Gives players feedback ("HOLD IT...")
- Creates tension moment

### Progressive Difficulty
- Rounds 1-2: Pure poses, no twists
- Rounds 3-5: Increasing twist probability
- Reaction time decreases per round
- Keeps early rounds accessible, late rounds chaotic

### Nomination Power
- 5/5 score = pick next victim
- Creates social dynamics
- Revenge opportunities
- 0/5 loses this power (wheel shame)

---

## Key Design Decisions

1. **Hot Seat > 1v1**: More spectator engagement, clearer rules
2. **Full Body > Hands Only**: More variety, funnier poses
3. **SVG Silhouettes > Emojis**: Clearer instructions, better vibes
4. **Deadpan > Hype**: Matches party game irreverence
5. **Stage BG > Webcam BG**: Less awkward, more game show
6. **Progressive Twists**: Easy start, chaos escalates

---

## The Hook

Every round is a potential viral moment. You either:
- Nail it and look smug
- Choke and get roasted
- Get twist-fucked mid-pose
- Earn the right to make someone drink

The near-miss tension + social nomination = party gold.

---

*Last updated: v1.0 - Hot Seat Edition*
