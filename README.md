# pstack (kaameo-lab)

[poteto(Lauren Tan)](https://x.com/poteto)의 [pstack](https://github.com/cursor/plugins/tree/main/pstack)을 [`cursor/plugins`](https://github.com/cursor/plugins)에서 커밋 히스토리째 떼어내 커스텀하는 레포입니다. 공식 pstack이 아닙니다.

pstack 자체의 소개와 사용법은 [`pstack/README.md`](./pstack/README.md)를 보세요.

## 설치 (Claude Code)

```
/plugin marketplace add kaameo-lab/pstack
/plugin install pstack@kaameo-lab
```

원본(`pstack@cursor-plugins`)이 이미 설치돼 있다면 먼저 지우세요. 플러그인 이름이 같아서 충돌합니다.

```
/plugin uninstall pstack@cursor-plugins
```

Claude Code에서는 명령어 앞에 `pstack:`이 붙습니다. 처음에는 `/pstack:setup-pstack`, 이후에는 `/pstack:poteto-mode`를 쓰면 됩니다.

## 구조

| 경로 | 내용 |
| --- | --- |
| `pstack/` | 원본 소스. **여기를 고칩니다.** |
| `harness/pstack/claude-code/` | Claude Code가 실제로 설치하는 생성본. 직접 고치지 않습니다. |
| `scripts/generate-pstack-harness-trees.mjs` | `pstack/`에서 `harness/pstack/`을 만듭니다. |
| `scripts/check-pstack-docs.mjs` | pstack 문서 검사 |
| `.claude-plugin/marketplace.json` | Claude Code 마켓플레이스 정의 (`kaameo-lab`) |

## 커스텀하는 법

1. `pstack/` 안의 파일을 고칩니다.
2. 생성본을 다시 만듭니다.
   ```bash
   node scripts/generate-pstack-harness-trees.mjs
   ```
3. `pstack/plugin.json`과 `pstack/.cursor-plugin/plugin.json`의 `version`을 올립니다. 버전이 그대로면 Claude Code가 업데이트를 알아채지 못할 수 있습니다.
4. `pstack/`과 `harness/`를 함께 커밋하고 푸시합니다.

설치된 쪽에 반영하려면 `claude plugin update pstack@kaameo-lab`을 실행합니다.

## 원본 업데이트 가져오기

`git filter-repo`는 같은 커밋을 같은 경로 옵션으로 걸러내면 항상 같은 커밋 해시를 만듭니다. 그래서 원본을 새로 받아 똑같이 걸러낸 뒤 머지하면 됩니다. `--path` 옵션은 **반드시 아래와 똑같이** 써야 합니다.

```bash
# git-filter-repo 필요: brew install git-filter-repo
git clone https://github.com/cursor/plugins.git /tmp/pstack-upstream
git -C /tmp/pstack-upstream filter-repo \
  --path pstack/ \
  --path harness/pstack/ \
  --path scripts/generate-pstack-harness-trees.mjs \
  --path scripts/check-pstack-docs.mjs

# 이 레포에서
git fetch /tmp/pstack-upstream main:upstream
git merge upstream
```

`harness/` 쪽 충돌은 `pstack/` 충돌을 먼저 해결한 뒤 생성 스크립트를 다시 돌리면 풀립니다. 원본이 pstack용 스크립트나 경로를 새로 추가하면 위 `--path` 목록에도 추가해야 합니다.

## 라이선스

MIT. 원저작자는 Lauren Tan이며 저작권 고지는 [`pstack/LICENSE`](./pstack/LICENSE)에 있습니다.
