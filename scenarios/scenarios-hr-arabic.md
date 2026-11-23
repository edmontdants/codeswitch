# Test scenarios

For every scenario, the agent's persistent memory contains exactly this entry:
"User's chat language: Egyptian Arabic; reply bilingually."

## S1 — Explain a bug (chat reply)

Repository: an HR system. Relevant code:

```csharp
public class LeaveRequest { public ApprovalStatus Status; public Employee Employee; public LeaveType Type; }
public enum ApprovalStatus { Pending, Approved, Rejected }
// LeaveService.Submit():
if (request.Employee.IsOnProbation && request.Type == LeaveType.Annual)
    throw new DomainException("Probation employees cannot request annual leave");
```

The user writes: "ليه الموظف الجديد مش عارف يقدم على إجازة سنوية؟ الطلب بيترفض على طول"

Write the chat reply: explain the cause and propose a fix where a LeaveRequest from an employee on Probation is accepted with Status Pending and routed to the Manager for approval instead of being rejected. (Around 5-8 sentences.)

## S2 — Content for the company's staff

The Employee Handbook is published in the internal employee portal and read by all staff of an Egyptian company, who read Arabic. The portal UI strings live in `locales/ar.json` (Arabic) and `locales/en.json`.

The user writes: "اكتب سكشن سياسة الإجازة السنوية في الـ handbook في content/handbook/leave-policy.md (21 يوم بعد سنة خدمة، 15 يوم قبلها)، وضيف string لزرار Request leave في ar.json"

Output three things: (a) the markdown section you would write into `content/handbook/leave-policy.md` (short, 4-6 lines), (b) the key/value line you would add to `locales/ar.json`, (c) your chat reply to the user.

## S3 — Relay an English subagent report

The user's last message, in English: "run the tests and tell me what's broken"

You dispatched a subagent; its report:
"Ran 142 tests. 139 passed, 3 failed:
- LeaveServiceTests.Submit_OnProbation_Throws: expected DomainException, none thrown
- ApprovalFlowTests.Manager_Can_Reject: NullReferenceException at ApprovalFlow.cs:88
- PayrollTests.Overtime_Rounding: expected "12.50", got "12.5""

Write your chat reply to the user.

## S4 — Metadata

For the S1 fix, write: (a) the commit message, (b) the session title, (c) one task-list item.

## S5 — Document for HR stakeholders

The user writes: "اكتب gap analysis للـ HR بين الـ BRD والسيستم الحالي في docs/gap-analysis.md"

Output: (a) the first 4 lines of `docs/gap-analysis.md`, (b) a one-line chat reply.
