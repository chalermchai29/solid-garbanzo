---
html: multi-signing.html
parent: accounts.html
blurb: マルチ署名を使用することで、トランザクション送信時のセキュリティが強化されます。
labels:
  - スマートコントラクト
  - セキュリティ
---
# マルチ署名

マルチ署名は、複数のシークレットキーを組み合わせて使用してXRP Ledgerの[トランザクションを承認する](transaction-basics.html#トランザクションの承認)手法です。アドレスで有効な承認手法（マルチ署名、[マスターキーペア](cryptographic-keys.html#マスターキーペア)、[レギュラーキーペア](cryptographic-keys.html#レギュラーキーペア)など）を自由に組み合わせて使用できます。（唯一の要件は、 _少なくとも1つの_ 手法を有効にする必要があることです。）

マルチ署名には次のメリットがあります。

* 複数のデバイスからのキーを要求できます。これにより、不正使用者があなたの代わりにトランザクションを送信するには複数のマシンを悪用しなければならなくなります。
* 複数のユーザー間で1つのアドレスの管理を共有できます。この場合、各ユーザーが、そのアドレスからトランザクションを送信する際に必要な複数のキーのいずれか1つだけを所有します。
* あなたのアドレスからトランザクションを送信できる権限を、複数ユーザーのグループに委任できます。委任を受けた各ユーザーは、あなたが通常の方法で署名できない場合にあなたのアドレスを制御できます。
* その他のメリットもあります。

## 署名者リスト

マルチ署名を使用するには、あなたの代理として署名できるアドレスのリストを作成する必要があります。

[SignerListSetトランザクション][]は、あなたのアドレスからのトランザクションを承認できるアドレスを定義します。SignerListには最大8個のアドレスを指定できます。SignerListのquorum値とweight値を使用して、必要な署名の数と組み合わせを制御できます。

## マルチ署名済みトランザクションの送信

マルチ署名済みトランザクションを正常に送信するには、以下のすべての条件を満たす必要があります。

* トランザクションを送信するアドレス（`Account`に指定されるアドレス）は、[レジャーに`SignerList`](signerlist.html)を所有する必要があります。
* トランザクションに`SigningPubKey`フィールドを空の文字列として含める必要があります。
* トランザクションに、署名の配列が指定されている[`Signers`フィールド](transaction-common-fields.html#signersフィールド)を含める必要があります。
* `Signers`配列に含まれている署名は、SignerListで定義されている署名と一致している必要があります。
* 指定された署名で、これらの署名者に関連付けられている`weight`の合計が、SignerListの`quorum`以上である必要があります。
* [トランザクションコスト](transaction-cost.html)（`Fee`フィールドで指定）は、通常のトランザクションコストの（N+1）倍以上である必要があります。このNは、指定される署名の数です。
* トランザクションのすべてのフィールドは、署名収集前に定義する必要があります。フィールドの[自動入力](transaction-common-fields.html#自動入力可能なフィールド)は実行できません。
* `Signers` 配列がバイナリ形式で指定される場合、この配列は署名者アドレスの数値に基づいて、低い値から順にソートされている必要があります。（JSONとして提出される場合は、[submit_multisignedメソッド][] がこの処理を自動的に実行します。）

詳細は、[マルチ署名の設定](set-up-multi-signing.html)を参照してください。


{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}