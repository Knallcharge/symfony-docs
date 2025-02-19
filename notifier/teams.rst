.. index::
    single: Notifier; Chatters

Microsoft Teams Notifier
========================

The Microsoft Teams Notifier package allows to use Microsoft Teams via the Symfony
Notifier component. Read the :doc:`main Notifier docs </notifier>` to learn about
installing and configuring that component.

Adding text to a Message
------------------------

With a Microsoft Teams, you can use the ChatMessage class::

    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\MicrosoftTeamsTransport;
    use Symfony\Component\Notifier\Message\ChatMessage;

    $chatMessage = (new ChatMessage('Contribute To Symfony'))->transport('microsoftteams');
    $chatter->send($chatMessage);

The result will be something like:

.. image:: /_images/notifier/microsoft_teams/message.png
   :align: center

Adding Interactions to a Message
--------------------------------

With a Microsoft Teams Message, you can use the
:class:`Symfony\\Component\\Notifier\\Bridge\\MicrosoftTeams\\MicrosoftTeamsOptions` class
to add `MessageCard options`_::

    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Action\ActionCard;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Action\HttpPostAction;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Action\Input\DateInput;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Action\Input\TextInput;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\MicrosoftTeamsOptions;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\MicrosoftTeamsTransport;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Section\Field\Fact;
    use Symfony\Component\Notifier\Bridge\MicrosoftTeams\Section\Section;
    use Symfony\Component\Notifier\Message\ChatMessage;

    $chatMessage = new ChatMessage('');

    // Action elements
    $input = new TextInput();
    $input->id('input_title');
    $input->isMultiline(true)->maxLength(5)->title('In a few words, why would you like to participate?');

    $inputDate = new DateInput();
    $inputDate->title('Proposed date')->id('input_date');

    // Create Microsoft Teams MessageCard
    $microsoftTeamsOptions = (new MicrosoftTeamsOptions())
        ->title('Symfony Online Meeting')
        ->text('Symfony Online Meeting are the events where the best developers meet to share experiences...')
        ->summary('Summary')
        ->themeColor('#F4D35E')
        ->section((new Section())
            ->title('Talk about Symfony 5.3 - would you like to join? Please give a shout!')
            ->fact((new Fact())
                ->name('Presenter')
                ->value('Fabien Potencier')
            )
            ->fact((new Fact())
                ->name('Speaker')
                ->value('Patricia Smith')
            )
            ->fact((new Fact())
                ->name('Duration')
                ->value('90 min')
            )
            ->fact((new Fact())
                ->name('Date')
                ->value('TBA')
            )
        )
        ->action((new ActionCard())
            ->name('ActionCard')
            ->input($input)
            ->input($inputDate)
            ->action((new HttpPostAction())
                ->name('Add comment')
                ->target('http://target')
            )
        )
    ;

    // Add the custom options to the chat message and send the message
    $chatMessage->options($microsoftTeamsOptions);
    $chatter->send($chatMessage);

The result will be something like:

.. image:: /_images/notifier/microsoft_teams/message-card.png
   :align: center

.. _`MessageCard options`: https://docs.microsoft.com/en-us/outlook/actionable-messages/message-card-reference
