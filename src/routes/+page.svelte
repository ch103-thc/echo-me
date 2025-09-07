<script>
	import { onMount } from 'svelte';
	import Icon from '@iconify/svelte';

	let username = $state('visitor');
	let input = $state('');
	let terminalBody;
	let inputElement;
	let response;

	let history = $state([
		{
			type: 'welcome',
			content: 'Type help to see the list of available commands.'
		}
	]);

	const welcomeMessage = $state([
		{
			type: 'welcome',
			content: 'Type help to see the list of available commands.'
		}
	]);

	const commands = $state([
		{
			cmd: 'about',
			desc: 'Learn more about who I am and what I do.',
			category: 'portfolio',
			icon: 'mdi:account'
		},
		{
			cmd: 'skills',
			desc: 'See the programming languages and tools I work with.',
			category: 'portfolio',
			icon: 'mdi:tools'
		},
		{
			cmd: 'projects',
			desc: "Explore some of the software projects I've built.",
			category: 'portfolio',
			icon: 'mdi:code-braces'
		},
		{
			cmd: 'contact',
			desc: 'Find out how to reach me.',
			category: 'portfolio',
			icon: 'mdi:email'
		},
		{
			cmd: 'clear',
			desc: 'Clear the terminal screen.',
			category: 'portfolio',
			icon: 'mdi:delete'
		},
		{
			cmd: 'help',
			desc: 'Display this help menu.',
			category: 'portfolio',
			icon: 'mdi:help-circle'
		}
	]);

	const projectList = [
		{
			name: 'Real-time Chat Application',
			stack: 'Node.js, Socket.io',
			description:
				'A scalable chat app supporting real-time communication with rooms and private messaging.',
			url: 'https://github.com/charlotte/realtime-chat',
			status: 'Complete',
			features: ['Real-time messaging', 'User rooms', 'Private chats', 'File sharing']
		},
		{
			name: 'E-commerce Web App',
			stack: 'React, Redux',
			description:
				'A fully functional storefront with product listings, cart, and user authentication.',
			url: 'https://github.com/charlotte/ecommerce-app',
			status: 'Active',
			features: ['Product search', 'User accounts', 'Payment processing', 'Order history']
		},
		{
			name: 'Cloud-based Data Storage System',
			stack: 'AWS, Lambda, DynamoDB',
			description: 'A secure and cost-effective cloud data storage solution with API endpoints.',
			url: 'https://github.com/charlotte/cloud-storage',
			status: 'Active',
			features: ['Secure storage', 'RESTful API', 'User permissions', 'File versioning']
		},
		{
			name: 'Machine Learning Model for Data Analytics',
			stack: 'Python, TensorFlow',
			description: 'A predictive model trained on large datasets to generate business insights.',
			url: 'https://github.com/charlotte/ml-analytics',
			status: 'In Development',
			features: [
				'Predictive analytics',
				'Custom dataset training',
				'Visualization tools',
				'API integration'
			]
		}
	];

	// Force scroll to bottom after content changes
	function scrollToBottom() {
		if (terminalBody) {
			setTimeout(() => {
				terminalBody.scrollTop = terminalBody.scrollHeight;
			}, 0);
		}
	}

	// Process command input
	function processCommand() {
		const trimmedInput = input.trim();
		if (!trimmedInput) return;

		const lowerInput = trimmedInput.toLowerCase();

		response = null;

		// Add the command input to history
		history = [
			...history,
			{ type: 'command', content: `${username}@portfolio:~$ ${trimmedInput}` }
		];

		switch (lowerInput) {
			case 'help':
				history = [
					...history,
					{
						type: 'help',
						content: {
							version: '1.0.0',
							categories: [
								{
									name: 'portfolio',
									label: 'Portfolio Commands',
									description: 'Learn more about my work and skills'
								}
							]
						}
					}
				];
				break;

			case 'about':
				history = [
					...history,
					{
						type: 'about',
						content: {
							name: 'Charlotte',
							title: 'Software Engineer',
							location: 'Los Angeles',
							summary:
								"I specialize in building scalable web applications, backend systems, and cloud-based solutions. I'm passionate about solving complex problems and creating efficient, maintainable software.",
							experience: '5+ years of professional development experience',
							interests: ['Web Development', 'Cloud Architecture', 'Open Source']
						}
					}
				];
				input = '';
				scrollToBottom();
				return;

			case 'skills':
				history = [
					...history,
					{
						type: 'skills',
						content: {
							categories: [
								{
									name: 'Languages',
									skills: ['JavaScript', 'TypeScript', 'Python']
								},
								{
									name: 'Frontend',
									skills: ['React', 'HTML/CSS', 'Svelte']
								},
								{
									name: 'Backend',
									skills: ['Node.js', 'Django', 'Express']
								},
								{
									name: 'DevOps & Cloud',
									skills: ['AWS', 'Docker', 'Kubernetes', 'CI/CD']
								},
								{
									name: 'Databases',
									skills: ['SQL', 'MongoDB', 'Redis']
								}
							]
						}
					}
				];
				input = '';
				scrollToBottom();
				return;

			case 'projects':
				history = [...history, { type: 'projects', content: 'Here are my latest projects:' }];

				// Add projects one by one with a special type
				projectList.forEach((project) => {
					history.push({ type: 'project', content: project });
				});

				input = '';
				scrollToBottom();
				return;

			case 'contact':
				history = [
					...history,
					{
						type: 'contact',
						content: {
							email: {
								value: 'charlotte@gmail.com',
								icon: 'mdi:email'
							},
							github: {
								value: 'https://github.com/c_harlotte',
								icon: 'mdi:github'
							},
							linkedin: {
								value: 'https://www.linkedin.com/in/c_harlotte',
								icon: 'mdi:linkedin'
							},
							instagram: {
								value: 'https://www.instagram.com/c_harlottedev/',
								icon: 'fa:instagram'
							},
							website: {
								value: 'https://charlotte.dev',
								icon: 'mdi:web'
							}
						}
					}
				];
				input = '';
				scrollToBottom();
				return;

			case 'clear':
				history = [...welcomeMessage];
				input = '';
				return;

			default:
				response = `Command not found: ${trimmedInput}`;
		}

		if (response) {
			history = [...history, { type: 'output', content: response }];
		}

		input = '';
		scrollToBottom();
	}

	// Handle key presses
	function handleKeyPress(e) {
		if (e.key === 'Enter') {
			processCommand();
		}
	}

	// Auto-focus on mount and after clicks
	onMount(() => {
		inputElement.focus();
		scrollToBottom();
	});

	function handleContainerClick() {
		inputElement.focus();
	}

	// Reactive statement to handle scroll when history changes
	$effect(() => {
		// Watch history changes
		history;
		scrollToBottom();
	});
</script>

<div class="terminal-body" bind:this={terminalBody} onclick={handleContainerClick}>
	<div class="history">
		{#each history as item}
			{#if item.type === 'help'}
				<div class="history-item help-item">
					<div class="help-header">
						<span class="help-title">Terminal Help</span>
						<span class="help-version">v{item.content.version}</span>
					</div>

					{#each item.content.categories as category}
						<div class="help-category">
							<div class="category-header">
								<span class="category-name">{category.label}</span>
							</div>
							<div class="category-desc">{category.description}</div>

							<div class="command-list">
								{#each commands.filter((cmd) => cmd.category === category.name) as cmd}
									<div class="command-item">
										<div class="command-icon">
											<Icon icon={cmd.icon} width="20" height="20" />
										</div>
										<div class="command-name">
											<span class="help-command">{cmd.cmd}</span>
										</div>
										<div class="command-desc">{cmd.desc}</div>
									</div>
								{/each}
							</div>
						</div>
					{/each}

					<div class="help-footer">
						Type any command to begin, or use the <span class="help-command">help</span> command to see
						this list again.
					</div>
				</div>
			{:else if item.type === 'welcome'}
				<div class="history-item output">
					<div>
						Type
						<span class="help-command">help</span> to see the list of available commands.
					</div>
				</div>
			{:else if item.type === 'about'}
				<div class="history-item about-item">
					<div class="about-header">
						<span class="about-name">{item.content.name}</span>
						<span class="about-title">{item.content.title}</span>
					</div>
					<div class="about-location">📍 {item.content.location}</div>
					<div class="about-summary">{item.content.summary}</div>
					<div class="about-experience">{item.content.experience}</div>
					<div class="about-interests">
						<span class="interests-label">Interests:</span>
						<div class="interests-tags">
							{#each item.content.interests as interest}
								<span class="interest-tag">{interest}</span>
							{/each}
						</div>
					</div>
				</div>
			{:else if item.type === 'skills'}
				<div class="history-item skills-item">
					<div class="skills-header">Technical Skills</div>
					<div class="skills-grid">
						{#each item.content.categories as category}
							<div class="skill-category">
								<div class="category-name">{category.name}</div>
								<div class="skills-list">
									{#each category.skills as skill}
										<span class="skill-tag">{skill}</span>
									{/each}
								</div>
							</div>
						{/each}
					</div>
				</div>
			{:else if item.type === 'project'}
				<div class="history-item project-item">
					<div class="project-header">
						<span class="project-name">
							<a href={item.content.url} target="_blank" rel="noopener" class="project-link">
								{item.content.name}
							</a>
						</span>
						<span class="project-status {item.content.status.toLowerCase().replace(' ', '-')}">
							{item.content.status}
						</span>
					</div>

					<div class="project-tech">
						<span class="tech-label">Tech:</span>
						{item.content.stack}
					</div>

					<div class="project-desc">
						{item.content.description}
					</div>

					<div class="project-features">
						<span class="features-label">Features:</span>
						<ul class="features-list">
							{#each item.content.features as feature}
								<li>{feature}</li>
							{/each}
						</ul>
					</div>
				</div>
			{:else if item.type === 'contact'}
				<div class="history-item contact-item">
					<div class="contact-header">Get In Touch</div>
					<div class="contact-links">
						{#each Object.entries(item.content) as [key, contactInfo]}
							<div class="contact-method">
								<span class="contact-icon">
									<Icon icon={contactInfo.icon} width="20" height="20" />
								</span>
								<a
									href={key === 'email' ? `mailto:${contactInfo.value}` : contactInfo.value}
									target={key === 'email' ? '' : '_blank'}
									rel="noopener"
									class="contact-link"
								>
									{key === 'email' ? contactInfo.value : key.charAt(0).toUpperCase() + key.slice(1)}
								</a>
							</div>
						{/each}
					</div>
				</div>
			{:else}
				<div
					class="history-item {item.type === 'error'
						? 'error'
						: item.type === 'command'
							? 'command'
							: 'output'}"
				>
					{#each item.content.split('\n') as line}
						<div>{line}</div>
					{/each}
				</div>
			{/if}
		{/each}
	</div>
	<div class="input-line">
		<span class="prompt">{username}@portfolio:~$</span>
		<input
			bind:this={inputElement}
			type="text"
			bind:value={input}
			onkeydown={handleKeyPress}
			class="terminal-input"
		/>
	</div>
</div>

<style lang="scss">
	.terminal-body {
		height: 25rem;
		overflow-y: scroll;
		color: var(--text);
		scrollbar-width: thin;
		scrollbar-color: var(--scrollbar-thumb) var(--darker);
		font-family: Menlo, Monaco, monospace;
		font-size: 1rem;

		/* For WebKit browsers (Chrome, Safari) */
		&::-webkit-scrollbar {
			width: 0.5rem;
		}

		&::-webkit-scrollbar-track {
			background-color: var(--darker);
		}

		&::-webkit-scrollbar-thumb {
			background-color: var(--scrollbar-thumb);
			border-radius: 0.25rem;
		}
	}

	.history-item {
		margin-bottom: 0.5rem;
		white-space: pre-wrap;

		&.command {
			color: var(--command);
		}

		&.output {
			color: var(--text);
		}

		&.error {
			color: var(--error);
		}
	}

	.help-command {
		color: var(--help);
		font-weight: bold;
		background-color: rgba(255, 183, 197, 0.1);
		padding: 0.1rem 0.4rem;
		border-radius: 0.25rem;
		font-family: 'Courier New', monospace;
	}

	.input-line {
		display: flex;
		margin-top: 0.5rem;

		.prompt {
			color: var(--command);
			font-weight: bold;
			margin-right: 0.5rem;
		}

		.terminal-input {
			background-color: transparent;
			border: none;
			color: var(--text);
			font-family: Menlo, Monaco, monospace;
			flex-grow: 1;
			outline: none;
			padding: 0;
			font-size: 1rem;
		}
	}

	/* Help section styles */
	.help-item {
		margin-bottom: 1rem;
		padding: 0.75rem;
		border-left: 2px solid var(--header);
		background-color: rgba(255, 255, 255, 0.03);

		.help-header {
			display: flex;
			justify-content: space-between;
			align-items: center;
			// margin-bottom: 1rem;
			padding-bottom: 0.5rem;
			border-bottom: 1px solid rgba(255, 255, 255, 0.1);

			.help-title {
				font-weight: bold;
				font-size: 1.2rem;
				color: var(--header);
			}

			.help-version {
				font-size: 0.8rem;
				color: var(--small-text);
				padding: 0.1rem 0.5rem;
				background-color: rgba(255, 255, 255, 0.05);
				border-radius: 1rem;
			}
		}

		.help-category {
			margin-bottom: 1.25rem;

			.category-header {
				margin-bottom: -1rem;

				.category-name {
					font-weight: bold;
					color: var(--text);
					font-size: 1.05rem;
				}
			}

			.category-desc {
				color: var(--small-text);
				font-size: 0.9rem;
				font-style: italic;
			}
		}

		.command-list {
			display: flex;
			flex-direction: column;
			// gap: 0.5rem;

			.command-item {
				display: grid;
				grid-template-columns: 24px minmax(80px, auto) 1fr;
				gap: 0.75rem;
				align-items: center;
				padding: 0.4rem 0.5rem;
				border-radius: 0.25rem;
				transition: background-color 0.2s;

				&:hover {
					background-color: rgba(255, 255, 255, 0.05);
				}

				.command-icon {
					font-size: 1.1rem;
					display: flex;
					align-items: center;
					justify-content: center;
				}

				.command-desc {
					color: var(--text);
				}
			}
		}

		.help-footer {
			margin-top: -1rem;
			padding-top: 0.5rem;
			border-top: 1px solid rgba(255, 255, 255, 0.1);
			font-size: 0.9rem;
			color: var(--small-text);
			text-align: center;
		}
	}

	/* About section styles */
	.about-item {
		margin-bottom: 1rem;
		padding: 0.5rem;
		border-left: 2px solid var(--header);
		background-color: rgba(255, 255, 255, 0.03);

		.about-header {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin-bottom: 0.5rem;

			.about-name {
				font-weight: bold;
				font-size: 1.2rem;
				color: var(--header);
			}

			.about-title {
				color: var(--text);
				font-style: italic;
			}
		}

		.about-location {
			font-size: 0.9rem;
			// margin-bottom: 0.5rem;
			color: var(--small-text);
		}

		// .about-summary {
		//   margin-bottom: 0.5rem;
		// }

		.about-experience {
			font-size: 0.9rem;
			color: var(--text);
			margin-bottom: 0.5rem;
		}

		.about-interests {
			.interests-label {
				color: var(--small-text);
				font-size: 0.9rem;
			}

			.interests-tags {
				display: flex;
				flex-wrap: wrap;
				gap: 0.5rem;
				margin-top: 0.25rem;

				.interest-tag {
					background-color: rgba(147, 197, 253, 0.2);
					color: var(--header);
					padding: 0.15rem 0.5rem;
					border-radius: 0.25rem;
					font-size: 0.8rem;
				}
			}
		}
	}

	/* Skills section styles */
	.skills-item {
		margin-bottom: 1rem;
		padding: 0.5rem;
		border-left: 2px solid var(--header);
		background-color: rgba(255, 255, 255, 0.03);

		.skills-header {
			font-size: 1.1rem;
			font-weight: bold;
			color: var(--header);
		}

		.skills-grid {
			display: grid;
			grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
			gap: 1rem;

			.skill-category {
				margin-bottom: 0.5rem;

				.category-name {
					font-weight: bold;
					margin-bottom: -0.8rem;
					color: var(--text);
				}

				.skills-list {
					display: flex;
					flex-wrap: wrap;
					gap: 0.5rem;

					.skill-tag {
						background-color: rgba(147, 197, 253, 0.1);
						color: var(--text);
						padding: 0.15rem 0.5rem;
						border-radius: 0.25rem;
						font-size: 0.8rem;
						border: 1px solid rgba(147, 197, 253, 0.3);
					}
				}
			}
		}
	}

	/* Contact section styles */
	.contact-item {
		margin-bottom: 1rem;
		padding: 1rem;
		border-left: 2px solid var(--header);
		background-color: rgba(255, 255, 255, 0.03);

		.contact-header {
			font-size: 1.1rem;
			font-weight: bold;
			margin-bottom: 0.75rem;
			color: var(--header);
			text-align: center;
			padding-bottom: 0.5rem;
			border-bottom: 1px solid rgba(255, 255, 255, 0.1);
		}

		.contact-links {
			display: grid;
			grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
			gap: 1rem;

			/* Mobile responsiveness */
			@media (max-width: 768px) {
				grid-template-columns: 1fr;
				gap: 0.75rem;
			}

			@media (max-width: 480px) {
				gap: 0.5rem;
			}

			.contact-method {
				display: flex;
				align-items: center;
				gap: 0.75rem;
				padding: 0.75rem;
				background-color: rgba(255, 255, 255, 0.05);
				border-radius: 0.5rem;
				transition: all 0.2s ease-in-out;

				/* Mobile adjustments */
				@media (max-width: 480px) {
					padding: 0.5rem;
					gap: 0.5rem;
				}

				&:hover {
					transform: translateY(-2px);
					background-color: rgba(255, 255, 255, 0.08);
					box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);

					/* Disable hover effects on mobile */
					@media (max-width: 768px) {
						transform: none;
					}
				}

				.contact-icon {
					font-size: 1.25rem;
					color: var(--header);
					background-color: rgba(147, 197, 253, 0.1);
					padding: 0.5rem;
					border-radius: 50%;
					display: flex;
					justify-content: center;
					align-items: center;
					width: 2.5rem;
					height: 2.5rem;
					flex-shrink: 0; /* Prevent icon from shrinking */

					/* Mobile adjustments */
					@media (max-width: 480px) {
						width: 2rem;
						height: 2rem;
						padding: 0.4rem;
					}
				}

				.contact-link {
					color: var(--text);
					text-decoration: none;
					font-size: 0.95rem;
					transition: color 0.2s;
					word-break: break-all; /* Handle long URLs on mobile */

					/* Mobile adjustments */
					@media (max-width: 480px) {
						font-size: 0.85rem;
					}

					&:hover {
						color: var(--header);
					}
				}
			}
		}
	}

	/* Project section styles */
	.project-item {
		margin-bottom: 1rem;
		padding: 0.5rem;
		border-left: 2px solid var(--header);
		background-color: rgba(255, 255, 255, 0.03);

		/* Mobile padding adjustment */
		@media (max-width: 480px) {
			padding: 0.75rem 0.5rem;
		}

		.project-header {
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin-bottom: 0.25rem;
			flex-wrap: wrap;
			gap: 0.5rem;

			/* Mobile stacking */
			@media (max-width: 480px) {
				flex-direction: column;
				align-items: flex-start;
				gap: 0.25rem;
			}

			.project-name {
				font-weight: bold;
				font-size: 1.1rem;

				/* Mobile font size */
				@media (max-width: 480px) {
					font-size: 1rem;
				}
			}

			.project-status {
				font-size: 0.8rem;
				padding: 0.1rem 0.4rem;
				border-radius: 0.25rem;
				flex-shrink: 0;

				/* Mobile adjustments */
				@media (max-width: 480px) {
					font-size: 0.7rem;
					padding: 0.05rem 0.3rem;
					align-self: flex-start;
				}

				&.complete {
					background-color: var(--complete);
					color: var(--tag);
				}

				&.active {
					background-color: var(--active);
					color: var(--tag);
				}

				&.in-development {
					background-color: var(--in-dev);
					color: var(--tag);
				}
			}
		}

		.project-tech {
			font-size: 0.9rem;
			margin-bottom: -1rem;

			/* Mobile font size */
			@media (max-width: 480px) {
				font-size: 0.8rem;
			}

			.tech-label {
				color: var(--small-text);
			}
		}

		.project-desc {
			line-height: 1.4;

			/* Mobile adjustments */
			@media (max-width: 480px) {
				font-size: 0.9rem;
				line-height: 1.5;
			}
		}

		.project-features {
			.features-label {
				color: var(--small-text);
				font-size: 0.9rem;

				/* Mobile font size */
				@media (max-width: 480px) {
					font-size: 0.8rem;
				}
			}

			.features-list {
				padding: 0;
				display: flex;
				flex-wrap: wrap;
				gap: 0.5rem 1rem;

				/* Mobile adjustments */
				@media (max-width: 480px) {
					gap: 0.25rem 0.75rem;
					flex-direction: column;
				}

				li {
					list-style-type: none;
					position: relative;
					padding-left: 0.75rem;
					font-size: 0.9rem;

					/* Mobile font size */
					@media (max-width: 480px) {
						font-size: 0.8rem;
						padding-left: 0.6rem;
					}

					&::before {
						content: '•';
						position: absolute;
						left: 0;
						color: var(--header);
					}
				}
			}
		}

		.project-link {
			color: var(--header);
			text-decoration: none;
			word-break: break-all; /* Handle long URLs */

			&:hover {
				text-decoration: underline;
			}
		}
	}
</style>
