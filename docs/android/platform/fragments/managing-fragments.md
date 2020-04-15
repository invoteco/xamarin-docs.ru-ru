---
title: Управление фрагментами
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027323"
---
# <a name="managing-fragments"></a>Управление фрагментами

Для упрощения управления фрагментами Android предоставляет класс `FragmentManager`. У каждого Действия есть экземпляр `Android.App.FragmentManager`, который будет находить или динамически изменять его Фрагменты. Каждый набор этих изменений называется *транзакция* и выполняется с помощью одного из интерфейсов API, содержащихся в классе `Android.App.FragmentTransation`, которым управляет `FragmentManager`. Действие может запустить транзакцию следующего вида:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Эти изменения Фрагментов выполняются в экземпляре `FragmentTransaction` при использовании таких методов, как `Add()`, `Remove(),` и `Replace().` Изменения затем применяются с помощью `Commit()`. Изменения в транзакции не выполняются немедленно.
Вместо этого их выполнение планируется в потоке пользовательского интерфейса Действия, как только это станет возможным.

В следующем примере показано, как добавить Фрагмент в существующий контейнер:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Если транзакцию будет зафиксировано после вызова `Activity.OnSaveInstanceState()`, вы увидите исключение. Это происходит, поскольку Действие сохраняет свое состояние, а Android также сохраняет состояние всех размещенных Фрагментов. Если после этой точки фиксируются какие-либо транзакции Фрагментов, состояние этих транзакций будет потеряно при восстановлении действия.

Транзакции фрагмента можно сохранить в [обратном стеке](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) Действия, вызвав `FragmentTransaction.AddToBackStack()`. Это дает возможность пользователю перемещаться назад по изменениям Фрагментов при нажатии кнопки **Назад**. Без обращения к этому методу удаляемые Фрагменты будут уничтожены и недоступны, если пользователь возвратится в Действие.

В следующем примере показано использование метода `AddToBackStack` `FragmentTransaction` для замены одного Фрагмента при сохранении состояния первого Фрагмента в обратном стеке:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```

## <a name="communicating-with-fragments"></a>Взаимодействие с Фрагментами

*FragmentManager* известно обо всех Фрагментах, присоединенных к действию, и поэтому для поиска этих Фрагментов он предоставляет два метода.

- **FindFragmentById** &ndash; этот метод будет искать Фрагмент с помощью идентификатора, указанного в файле макета, или идентификатор контейнера, если фрагмент добавлен как часть транзакции.

- **FindFragmentByTag** &ndash; этот метод используется для поиска Фрагмента с тегом, который был предоставлен в файле макета или добавлен в транзакцию.

На `FragmentManager`ссылаются и Фрагменты и Действия, поэтому для обмена данными между ними используются одни и те же методы. Используя один из этих двух методов, приложение может найти Фрагмент ссылки, придать этой ссылке соответствующий тип, а затем напрямую вызвать методы в Фрагменте. В качестве примера можно привести следующий фрагмент кода:

Кроме того, для поиска фрагментов Действие может использовать `FragmentManager`:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>Взаимодействие с Действием

Для ссылки на его узел Фрагмент может использовать свойство `Fragment.Activity`. Приведя Действие к более конкретному типу, оно сможет вызвать методы и свойства на своем узле, как показано в следующем примере:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
