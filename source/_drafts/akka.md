akka名词转换

ActorRef#tell(msg, sender) => ActorRef.told(msg, sender)
Inbox#send(target, msg)     => Inbox#internalActor.send(target, msg)
