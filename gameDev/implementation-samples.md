# Singleton implementation

Generic singleton class. Use this class whenever you need just one instance of gameObject in game, no matter of scene.

``` csharp
public abstract class UnitySingleton<T> : MonoBehaviour where T : Component
{
	#region Fields

	private static T _instance;

	#endregion

	#region Properties

	public static T Instance
	{
		get
		{
			if (_instance == null)
			{
				_instance = FindObjectOfType<T>();
				if (_instance == null)
				{
					// Create instance of gameObject and attach this script to it.
					GameObject obj = new GameObject();
					obj.name = typeof(T).Name;
					_instance = obj.AddComponent<T>();
				}
			}

			return _instance;
		}
	}

	#endregion

	#region Methods

	protected virtual void Awake()
	{
		// Make sure only one copy is persisted even during scene change.
		if (_instance == null)
		{
			_instance = this as T;
			DontDestroyOnLoad(gameObject);
		}
		else
		{
			Destroy(gameObject);
		}
	}

	#endregion
}
```
# Event system

`EventBase.cs`

``` csharp
/// <summary>
/// Base class for custom events.
/// </summary>
public abstract class EventBase
{
    /// <summary>
    /// Indicates that this event was processed and should not be passed to other receivers.
    /// </summary>
    public bool StopPropagation = false;
}
```

`EventManager`

``` csharp
public sealed class EventManager : UnitySingleton<EventManager>
{
	#region Fields

	/// <summary>
	/// Holds inner nodes with receivers to react on specific types of events.
	/// </summary>
	private readonly Dictionary<Type, List<EventSystemNode>> _nodes = new();

	#endregion

	#region Public methods

	/// <summary>
	/// Register handler action (method) for specified type of event.
	/// </summary>
	/// <typeparam name="T">Type of event to be handled by this handler</typeparam>
	/// <param name="action">Action (method) to be called when event raised</param>
	/// <param name="priority">Priority of this handler among list of all handlers</param>
	public void Register<T>(Action<T> action, int priority = 0) where T : EventBase
	{
		if (!_nodes.ContainsKey(typeof(T)))
			_nodes.Add(typeof(T), new List<EventSystemNode>());

		_nodes[typeof(T)].Add(EventSystemNode.Create(action, priority));
		_nodes[typeof(T)].Sort((node1, node2) => node1.Priority.CompareTo(node2.Priority));
	}

	/// <summary>
	/// Unregister handler action (method) for specified type of event.
	/// </summary>
	/// <typeparam name="T">Type of event to be handled by this handler</typeparam>
	/// <param name="action">Action (method) to be deregistered</param>
	public void Unregister<T>(Action<T> action) where T : EventBase
	{
		if (!_nodes.ContainsKey(typeof(T)))
			return;

		_nodes[typeof(T)].RemoveAll(e => (Action<T>)e.Action == action);

		if (!_nodes[typeof(T)].Any())
			_nodes.Remove(typeof(T));
	}

	/// <summary>
	/// Raise the specified event and call all handlers.
	/// </summary>
	/// <param name="e">Event to be raised</param>
	public void SendEvent(EventBase e)
	{
		if (!_nodes.ContainsKey(e.GetType()))
			return;

		foreach (EventSystemNode node in _nodes[e.GetType()])
		{
			if (e.StopPropagation)
				break;
			node.Callable(e);
		}
	}

	#endregion

	#region EventSystemNode class

	/// <summary>
	/// Inner node containing action registered to react on a specific event.
	/// </summary>
	private class EventSystemNode
	{
		/// <summary>
		/// Reference to action (method) which was registered to react on a specific event.
		/// Used to be able to deregister handler.
		/// </summary>
		public object Action { get; }

		/// <summary>
		/// Action to be called when specific event raised.
		/// </summary>
		public Action<EventBase> Callable { get; }

		/// <summary>
		/// Priority of this handler among other handlers.
		/// The higher number, the higher priority (first called).
		/// </summary>
		public int Priority { get; }

		private EventSystemNode(object action, Action<EventBase> callable, int priority)
		{
			Action = action;
			Callable = callable;
			Priority = priority;
		}

		/// <summary>
		/// Static factory method to encapsulate event Receiver method to a callable action to be called when event raised.
		/// </summary>
		/// <typeparam name="T">Type of event</typeparam>
		/// <param name="action">Receiver method to be called when event raised</param>
		/// <param name="priority">Priority of this handler</param>
		/// <returns>Inner node to be added to EventSystem</returns>
		public static EventSystemNode Create<T>(Action<T> action, int priority) where T : EventBase
		{
			Action<EventBase> callable = delegate(EventBase e) { action((T)e); };
			return new EventSystemNode(action, callable, priority);
		}
	}

	#endregion
}
```
