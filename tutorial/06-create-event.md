<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.

## <a name="add-method-to-graphservice"></a>Ajouter une méthode à GraphService

1. Ouvrez **./SRC/GraphService.TS** et ajoutez la fonction suivante pour créer un événement.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="createEventSnippet":::

## <a name="create-new-event-form"></a>Créer un formulaire d’événement

1. Créez un fichier dans le répertoire **./SRC** nommé **NewEvent. TSX** et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/NewEvent.tsx" id="NewEventSnippet":::

1. Ouvrez **./SRC/App.TSX** et ajoutez l' `import` instruction suivante en haut du fichier.

    ```typescript
    import NewEvent from './NewEvent';
    ```

1. Ajoutez un nouvel itinéraire dans le nouveau formulaire d’événement. Ajoutez le code suivant juste après les autres `Route` éléments.

    ```typescript
    <Route exact path="/newevent"
      render={(props) =>
        this.props.isAuthenticated ?
          <NewEvent {...props} /> :
          <Redirect to="/" />
      } />
    ```

    L' `return` instruction complète doit maintenant ressembler à ce qui suit.

    :::code language="typescript" source="../demo/graph-tutorial/src/App.tsx" id="renderSnippet" highlight="23-28":::

1. Actualisez l’application et naviguez jusqu’à l’affichage Calendrier. Cliquez sur le bouton **nouvel événement** . Renseignez les champs, puis cliquez sur **créer**.

    ![Capture d’écran du nouveau formulaire d’événement](./images/create-event-01.png)
