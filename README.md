# avantos-forms

## Installation

```
npm i https://github.com/mosaic-avantos/avantos-forms/releases/download/v0.0.2510/avantos-forms.tar
```

## Usage

### Submit/Save

```ts
import { useActionRunTaskRender } from '@/api/api.ts';
import { TaskForm, useAvantosSubmitForm, FormProvider } from 'avantos-forms';
import { useParams } from 'react-router-dom';

export function SubmitFormPage() {
  const { action_run_id, task_id } = useParams();
  if (!action_run_id || !task_id) {
    throw new Error('action_run_id and task_id are required');
  }
  return (
    <FormProvider>
      <SubmitForm actionRunId={action_run_id} taskId={task_id} />
    </FormProvider>
  );
}

function SubmitForm({
  actionRunId,
  taskId,
}: {
  actionRunId: string;
  taskId: string;
}) {
  const {
    saveMutate,
    saveIsPending,
    saveIsError,
    submitMutate,
    submitIsPending,
    submitIsError,
  } = useAvantosSubmitForm(
    actionRunId,
    taskId,
    tenantId,
  );
  const { data, isLoading, error } = useActionRunTaskRender(
    actionRunId,
    taskId,
  );
  if (isLoading || saveIsPending || submitIsPending) {
    return <div>Loading...</div>;
  }
  if (error || !data) {
    return <div>Error: {error?.message ?? 'No data!'}</div>;
  }

  return (
    <div>
      <TaskForm data={data} />
      {(saveIsError || submitIsError) && "error!"}
      <button onClick={saveMutate}>Save</button>
      <button onClick={submitMutate}>Submit</button>
    </div>
  );
}
```

### Approve/Reject

Use `useAvantosApproveForm` with `TaskForm` and `FormProvider` as above.

### Configuration Form

Use `useAvantosConfigurationForm` with `TaskForm` and `FormProvider` as above.

### Readonly

`TaskForm` takes a `readonly` prop that will render it in readonly mode (prohibits editing).


