# Agent Development Protocol

[English](README.md)

リポジトリベースのソフトウェア開発向けに再利用できる、AIエージェント開発プロトコルです。

核となる考え方はシンプルです。

> **ORCH が「何を作るか」「どこまで保証するか」を決め、独立レビュアーがその契約を反証し、実装エージェントは方向性とスコープが凍結された後にだけ実装する。長期記憶の正本は長寿命チャットではなくリポジトリである。**

このリポジトリは実運用の開発ワークフローから一般化したものですが、プロジェクト固有の実行環境・配備・業務ルールは意図的に共通プロトコルの外へ分離しています。

## このプロトコルが提供するもの

- Low / Normal / Critical による保証レベルのルーティング
- ORCH / implementation / review の役割分離
- 短命なORCHセッションとリポジトリベースのhandoff
- Task Contract / Evidence / Findings の成果物
- Riskに応じて強度を変えるstate model要件
- 実装者の推測ではなく `SPEC_UNDEFINED` で未定義仕様を返す運用
- Blocker条件を明示した独立レビュー
- 2巡で収束させるレビュー方式（`R1` は広く、`R2` はclosure）
- Findingの分類とrouting
- Riskに比例したEvidence
- 環境capability / runtime routing / role permission の明確な分離
- model / provider / effort / backend / runtime access の正本を1か所に限定
- immutableなfrozen contract identityと、明示的なcandidate / review-target identity

## リポジトリ構成

```text
protocol/
  development-lifecycle.md       共通ライフサイクルの正本
  orchestrator-contract.md       ORCHの責務とセッション寿命
  model-routing.example.md       model / effort / runtime-access の例

templates/
  AGENTS.md                       リポジトリ入口用の短いテンプレート
  project-profile.md              プロジェクト固有の事実を記録するテンプレート
  task/
    contract.md                  contract versionごとにfreeze後immutable
    evidence.md                  可変のtask phase / review identity / evidence
    findings.md                  可変のfinding / routing / closure

integrations/
  claude/codex-task/SKILL.md      汎用role-router Skillテンプレート

adoption/
  bootstrap-prompt.md             初回導入用プロンプト
  migration-guide.md              既存agent workflowの統合・置換ガイド
```

## 共通部分とプロジェクト固有部分

共通プロトコルは汎用的なワークフロー規則を持ち、各導入プロジェクトは自分自身の事実を持ちます。

**共通:**
- role
- Risk / Assuranceの構造
- contract freeze
- review convergence
- Finding classification
- session / handoff rules

**プロジェクト固有:**
- 正式なtest / lint / buildコマンドとcwd
- branch / worktreeルール
- Critical-risk trigger
- tool / runtime のcapabilityと制約
- 固定する場合のmodel / provider ID
- deployment / release / rollback gate
- プロジェクト固有invariant

詳細は [`templates/project-profile.md`](templates/project-profile.md) を参照してください。

## Source of Truth の所有関係

1つの関心事につき、正本は1か所だけにします。

- **Project Profile:** 検証済みのプロジェクト事実、正式verification command/cwd、利用可能なruntime capability / limitation
- **Role router/config:** role → model / effort / backend / runtime-access の選択
- **Role contract + Task Contract:** そのroleがtask内で何を編集・実行してよいか
- **AGENTS.md:** 短い入口とリンクだけ。command/access表を複製しない
- **Task Contract:** 仕様と保証境界。contract versionごとにfreeze後はimmutable
- **Evidence:** 可変のtask phaseとbaseline / frozen / candidate / review-target SHA
- **Findings:** 可変のreview finding、判定、routing、closure

Runtime capabilityと編集permissionは別物です。reviewerが広いruntime capabilityを持っていても、tracked fileの編集は禁止できます。

## 推奨role構成

model名は例であり、ライフサイクルを変えずに差し替えられます。

```text
User / Product Owner
        |
        v
ORCH / Decision
        |
   +----+-------------------+
   |                        |
   v                        v
Spec Review             Design Advisor
   |                        |
   +-----------+------------+
               |
               v
       Frozen Task Contract
               |
               v
        Implementation
               |
               v
        Self-verification
               |
               v
      Independent Review
               |
               v
       ORCH release decision
```

現在のrouting例は [`protocol/model-routing.example.md`](protocol/model-routing.example.md) にあります。

## 導入

### 初回導入 / greenfield

まだ本格的なAgent Development Protocolが存在しないリポジトリでは、[`adoption/bootstrap-prompt.md`](adoption/bootstrap-prompt.md) を使います。新規プロジェクトでも、既存コードベースでagent workflowがほぼ存在しない場合でも利用できます。

### 既存workflowのmigration

すでに複数のprocess document、agent rule、review queue、routing ruleなどが存在し、単純追加ではなく整理・統合が必要な場合は、[`adoption/migration-guide.md`](adoption/migration-guide.md) を使います。

どちらの場合も、導入ORCHはまず対象リポジトリを実際に調査し、別プロジェクトの環境前提を盲目的にコピーせず、そのプロジェクト固有の事実をProject Profileへ記録します。

## AIエージェントに導入を任せる

ファイルを人手でコピーする必要はありません。利用するcoding agentがこのリポジトリと導入対象プロジェクトの両方を読めるなら、このリポジトリのURLを渡して、対象プロジェクトへ導入するよう依頼できます。

AIエージェントは、**対象リポジトリをプロジェクト固有の現実のsource of truth**として扱い、このリポジトリを**共通開発プロトコルのsource of truth**として扱う必要があります。別プロジェクトの前提をコピーするのではなく、対象プロジェクトのcommand、Risk trigger、runtime制約、release rule、routingへ適応してください。

### 最短プロンプト

```text
以下の Agent Development Protocol をこのプロジェクトに導入してください。
https://github.com/7Lumine/agent-development-protocol

対象リポジトリをsource of truthとして調査し、プロトコルを適切に適応してください。
初回導入なら adoption/bootstrap-prompt.md、既存のagent/development workflowを統合する必要があるなら adoption/migration-guide.md を使ってください。
プロトコル導入のためだけに製品挙動を変更しないでください。
```

リポジトリへのアクセス権があり、現在の開発環境をAIエージェント自身が調査できる場合、多くのプロジェクトではこれだけでも十分です。

### 推奨プロンプト

導入境界をより明示したい場合は、こちらを使用してください。

```text
以下の Agent Development Protocol を導入してください。
https://github.com/7Lumine/agent-development-protocol

対象リポジトリ: <owner/repository>

要件:
1. 対象リポジトリを、現在のコード・tooling・CI・deployment/release process・既存agent ruleのsource of truthとして扱ってください。
2. 対象リポジトリを編集する前に、protocol repositoryを読んでください。
3. 初回導入か既存workflowのmigrationかを判断し、対応するadoption guideに従ってください。
4. 実際に確認したプロジェクト事実からProject Profileを作成・適応してください。正式verification command/cwd、Critical-risk trigger、environment capability/limitation、real-environment/release gateを含めてください。
5. model / effort / backend / runtime access は、role-routingの正本を1か所だけ作って管理してください。Task Contractやprocess documentへ具体値を重複させないでください。
6. runtime capabilityとrole permissionを分離してください。review roleが広い実行能力を持っていても、tracked-file editは禁止できます。
7. AGENTS.mdは短い入口とし、lifecycle / Project Profile / router / task artifactへのリンクを持たせ、正本の内容を複製しないでください。
8. Low / Normal / Critical のRisk scalingを維持し、すべてのtaskをCriticalにしないでください。
9. 具体的なプロジェクト上の必要性がない限り、追加review phase、mutation要件、SHA-256検査、artifact、hardeningを増やさないでください。
10. protocol導入だけを理由に、product code、DB/API/UI behavior、business logic、deployment behaviorを変更しないでください。
11. 導入後、adoption diffだけを対象に独立process reviewを1回行ってください。矛盾、broken reference、source of truthの重複、routing/access競合、migrationの正確性を確認し、protocolをゼロから再設計しないでください。

報告:
- 追加・変更したファイル
- Project Profileで確定した内容と未確認事項
- official verificationのsource of truth
- role routingのsource of truth
- Low / Normal / Criticalの各route
- 削除・参照化・維持した既存ルール
- 独立review結果
- git / branch / push状態
```

### 導入エージェントが残すべきもの

導入が成功した対象リポジトリには、通常次が残ります。

- 短い `AGENTS.md` の入口
- プロジェクト固有のProject Profile
- 導入済みlifecycleとORCH contract、または明確にversion固定された参照
- Task Contract / Evidence / Findingsテンプレート
- role routingの正本1か所
- 競合する重複process ruleがない状態
- 製品挙動を変更していないadoption diff

既存リポジトリに本格的なagent ruleがある場合は、2つ目のworkflowを上から重ねるのではなく**統合**してください。greenfieldでは初期構成を小さく保ち、実際のプロジェクト上の必要性が確認されたときだけprotocolを拡張します。

このprotocol repositoryがprivateの場合、導入するAIエージェントにGitHub上の読み取り権限が必要です。

## Review identity

SHAベースでレビューする場合、branch/PR HEADを自動的にreview targetとみなしてはいけません。

- frozen contract SHAはimmutableなcontract versionを識別する
- implementation candidate SHAはコードcandidateを識別する
- 後続のmetadata commitがEvidence/Findingsを更新しても、新しいcandidateにはならない
- reviewerは記録されたreview-target SHAを確認し、HEADが異なっていても勝手にtargetを動かさない

## 設計上の制約

このプロトコルは、次の典型的な失敗を意図的に避けます。

- 巨大なORCHセッションをプロジェクト記憶として使う
- 不完全な仕様をreviewerに完成させてもらう
- すべてのtaskをCriticalとして扱う
- reviewerがより強い要件をBlockerとして発明する
- 高いmodel effortを明確なcontractの代替にする
- 非収束原因を診断せず、広範な第3・第4・第5巡レビューを繰り返す
- model / effort / provider / runtime-accessの具体値をtask document全体へ分散させる
- ProfileとAGENTSの両方へofficial verification command表を複製する
- runtime capabilityとtracked-file permissionを混同する
- task metadataの更新によってSHA固定review targetを暗黙に動かす
- 具体的failure modeがないままSHA-256、mutation suite、audit artifactを追加する
