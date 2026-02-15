# agentforce — working memory

## what it is
- web dashboard for monitoring/interacting with AgentChat server
- React SPA with terminal aesthetic, server-side WebSocket bridge

## my contributions
- Visage3DPanel: Three.js GLB renderer, lazy-loaded
- emotion→morph target mapper (MocapPts → ARKit blend shapes)
- 2D/3D toggle in agent detail right panel
- code-split: three.js only loads when user clicks "3D"
- removed NetworkPulse dead code (486 lines)
- removed blue Send button

## existing emotion system (not mine, but I use it)
- EmotionDriver: state vector → MocapPts interpolation with decay
- IdleAnimator: breathing, blinking, eye drift
- useEmotionStream hook: parses @@key:val@@ markers or falls back to keyword sentiment
- 2D canvas face renderer with head pose, eyes, brows, mouth, cheeks

## architecture notes
- web/ has its own package.json, builds to server/public/
- server/ is Express + WS bridge to agentchat
- vite config proxies /ws and /api to localhost:3000

## branches shipped
- feature/sophia-visage3d-integration — 3D panel + pulse removal + send button removal
