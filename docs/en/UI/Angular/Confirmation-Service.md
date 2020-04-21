# ConfirmationService

You can use the `ConfirmationService` in @abp/ng.theme.shared package to place a confirmation popup at the root level.


## Getting Started

You do not have to provide the `ConfirmationService` at module or component level, because it is already **provided in root**. You can inject and start using it immediately in your components, directives, or services.


```js
import { ConfirmationService } from '@abp/ng.theme.shared';

@Component({
  /* class metadata here */
})
class DemoComponent {
  constructor(private confirmation: ConfirmationService) {}
}
```

## Usage

You can use the `success`, `warn`, `error`, and `info` methods of `ConfirmationService` to display a confirmation popup at the root level in your project.

### How to Display a Confirmation Popup

```js
const confirmationStatus$ = this.confirmation.success('Message', 'Title')
```

- The `ConfirmationService` methods accept three parameters that are `message`, `title`, and `options`.
- `success`, `warn`, `error`, and `info` methods return an [RxJS Subject](https://rxjs-dev.firebaseapp.com/guide/subject) to listen to confirmation popup closing event. The type of event value is [`Confirmation.Status`](https://github.com/abpframework/abp/blob/master/npm/ng-packs/packages/theme-shared/src/lib/models/confirmation.ts#L24) that is an enum.

### How to Listen Closing Event

You can subscribe the confirmation closing event like below:

```js
import { Confirmation, ConfirmationService } from '@abp/ng.theme.shared';

constructor(private confirmation: ConfirmationService) {}

this.confirmation
  .warn('::WillBeDeleted', { key: '::AreYouSure', defaultValue: 'Are you sure?' })
  .subscribe((status: Confirmation.Status) => {
    switch (status) {
      case Confirmation.Status.confirm:
        // confirmed, the popup was closed by clicking the confirm button
        break;
      case Confirmation.Status.reject:
        // rejected, the popup was closed by clicking the cancel button
        break;
      case Confirmation.Status.dismiss:
        // dismissed, the popup was closed by pressing the escape
        break;
    }
  });
```

- The `message` and `title` parameters accept a string, localization key or localization object. See the [localization document](./Localization.md)

If the status is not important for you, you may not listen the closing event:

```js
this.confirmation.error('You are not authorized.', 'Error');
```

### How to Display a Confirmation Popup With Given Options

Options can be passed as the third parameter to `success`, `warn`, `error`, and `info` methods:

```js
const options: Partial<Confirmation.Options> = {
  hideCancelBtn: false,
  hideYesBtn: false,
  cancelText: 'Close',
  yesText: 'Confirm',
  messageLocalizationParams: ['Demo'],
  titleLocalizationParams: [],
};

this.confirmation.warn(
  'AbpIdentity::RoleDeletionConfirmationMessage',
  'Are you sure?',
  options,
);
```

- `hideCancelBtn` option is the boolean value that allows the cancel button to be displayed or hidden. Default value is `false`
- `hideYesBtn` option is the boolean value that allows the confirm button to be displayed or hidden. Default value is `false`
- `cancelText` is the text of the cancel button. A localization key or localization object can be passed. Default value is `AbpUi::Cancel`
- `yesText` is the text of the confirm button. A localization key or localization object can be passed. Default value is `AbpUi::Yes`
- `messageLocalizationParams` is the interpolation parameters of the message localization.
- `titleLocalizationParams` is the interpolation parameters of the title localization.

With the above options, the confirmation popup looks like this:

![confirmation](./images/confirmation.png)

### How to Remove a Confirmation Popup

The open confirmation popup can be removed manually via the `clear` method:

```js
this.confirmation.clear();
```

## API

### success

```js
success(
  message: Config.LocalizationParam,
  title: Config.LocalizationParam,
  options?: Partial<Confirmation.Options>,
): Observable<Confirmation.Status>
```

> See the [`Config.LocalizationParam` type](https://github.com/abpframework/abp/blob/master/npm/ng-packs/packages/core/src/lib/models/config.ts#L46) and [`Confirmation` namespace](https://github.com/abpframework/abp/blob/master/npm/ng-packs/packages/theme-shared/src/lib/models/confirmation.ts)


### warn

```js
warn(
  message: Config.LocalizationParam,
  title: Config.LocalizationParam,
  options?: Partial<Confirmation.Options>,
): Observable<Confirmation.Status>
```

### error

```js
error(
  message: Config.LocalizationParam,
  title: Config.LocalizationParam,
  options?: Partial<Confirmation.Options>,
): Observable<Confirmation.Status>
```

### info

```js
info(
  message: Config.LocalizationParam,
  title: Config.LocalizationParam,
  options?: Partial<Confirmation.Options>,
): Observable<Confirmation.Status>
```

### clear

```js
clear(
  status: Confirmation.Status = Confirmation.Status.dismiss
): void
```

- `status` parameter is the value of the confirmation closing event.


## What's Next?

- ToasterService 
<!-- TODO: Add ToasterService link -->