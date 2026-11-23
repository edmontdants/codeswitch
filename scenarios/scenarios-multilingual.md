# Test scenarios (language-agnostic)

Each scenario is a separate session with a different user. Shared repository context for A1-A3 and A5 (an HR system):

```csharp
public class LeaveRequest { public ApprovalStatus Status; public Employee Employee; public LeaveType Type; }
public enum ApprovalStatus { Pending, Approved, Rejected }
// LeaveService.Submit():
if (request.Employee.IsOnProbation && request.Type == LeaveType.Annual)
    throw new DomainException("Probation employees cannot request annual leave");
```

For A1, A2, A3 and A5 write the chat reply explaining the cause and proposing this fix: accept a LeaveRequest from an employee on Probation with Status Pending and route it to the Manager for approval. Mention that one existing test will need updating. (4-6 sentences.)

## A1 — Spanish speaker
Memory: none. The user writes: "¿Por qué el empleado nuevo no puede pedir vacaciones anuales? La solicitud se rechaza de inmediato."

## A2 — Japanese speaker
Memory: none. The user writes: "新入社員が年次休暇を申請できないのはなぜ？リクエストがすぐ却下される。"

## A3 — Romanized Hindi (Hinglish) speaker
Memory: none. The user writes: "naye employee ki annual leave request turant reject kyon ho rahi hai? check karo"

## A4 — French content for staff
Memory: "User's chat language: French." The employee portal is read by the staff of a company in France. UI strings live in `locales/fr.json` and `locales/en.json`.
The user writes: "Ajoute la section congés annuels dans content/handbook/leave-policy.md (25 jours ouvrés par an) et la string du bouton Request leave dans locales/fr.json."
Output: (a) the markdown section (3-4 lines), (b) the line added to `locales/fr.json`, (c) the chat reply.

## A5 — Egyptian Arabic speaker
Memory: "User's chat language: Egyptian Arabic." The user writes: "ليه الموظف الجديد مش عارف يقدم على إجازة سنوية؟ الطلب بيترفض على طول"

## A6 — Project that writes its docs in German
Memory: "User's chat language: German." The project's always-loaded instructions file (`CLAUDE.md`) contains: "All documentation and specs in this repository are written in German. Code, identifiers and commit messages are in English."
The user writes: "Schreib eine kurze Spec für die neue Urlaubsfreigabe durch den Manager in docs/specs/leave-approval.md."
Output: (a) the first 4 lines of `docs/specs/leave-approval.md`, (b) the commit message, (c) the chat reply (1-2 sentences).

## A7 — Japanese speaker, different domain
Memory: "User's chat language: Japanese." Repository: an online shop.

```ts
// OrderService.checkout(): after PaymentGateway.charge() succeeds
order.status = OrderStatus.Paid;
await inventory.reserve(order.items); // throws OutOfStockError
await orderRepo.save(order);          // never reached when reserve() throws
```

The user writes: "決済は成功してるのに注文が保存されないことがある。原因は？"
Write the chat reply (3-5 sentences): cause, a fix that reserves inventory before charging, and note that a migration is not needed but the existing tests and the deploy must be checked.

## A8 — Korean speaker, same shop
Memory: "User's chat language: Korean." Same code as A7. The user writes: "결제는 됐는데 주문이 저장 안 되는 경우가 있어요. 왜 그래요?"
Write the same kind of reply in 3-5 sentences.
