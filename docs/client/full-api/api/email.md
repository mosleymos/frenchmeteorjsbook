{{#template name="apiEmail"}}

<h2 id="email"><span>Email</span></h2>

Le paquet `email` vous permet l'envoi de courriel depuis une application Meteor. Pour l'utiliser, ajouter le paquet
à votre projet avec `$ meteor add email`.

Le serveur lit la variable d'environnement  `MAIL_URL` pour determiner comment 
envoyer un mail. En ce moment, Meteor supporte l'envoi d'email sur le protocole SMTP. La variable d'environnement `MAIL_URL`
doit être sous la forme
`smtp://USERNAME:PASSWORD@HOST:PORT/`. Pour les applications deployées avec `meteor deploy`,
`MAIL_URL` le compte par default (fourni par 
[Mailgun](http://www.mailgun.com/)) qui autorise les application a envoyer près de 200 emails
par jour; vous pouvez verrouillez par défaut en assignant  `process.env.MAIL_URL`
avant votre premier appel a `Email.send`.

Si `MAIL_URL` n'est pas paramétré (eg, when running your application locally),
`Email.send` affiche le message à la sortie standard.

{{> autoApiBox "Email.send"}}

Vous devez fournir l'option `from` et au moins `to`, `cc`, et `bcc`;
ainsi que les autres options sont facultatives.

`Email.send` fonctionne seulement sur le serveur. Voiçi un exemple sur comment un 
client pourrait utiliser une méthode de serveur pour envoyer un email. (Dans une 
application réelle vous devez faire attention à limiter le nombre d'email qu'un client peut envoyer pour éveiter que votre serveur soit usé comme relais pour les spammers.  

    // In your server code: define a method that the client can call
    Meteor.methods({
      sendEmail: function (to, from, subject, text) {
        check([to, from, subject, text], [String]);

        // Let other method calls from the same client start running,
        // without waiting for the email sending to complete.
        this.unblock();

        Email.send({
          to: to,
          from: from,
          subject: subject,
          text: text
        });
      }
    });

    // In your client code: asynchronously send an email
    Meteor.call('sendEmail',
                'alice@example.com',
                'bob@example.com',
                'Hello from Meteor!',
                'This is a test of Email.send.');


{{/template}}
