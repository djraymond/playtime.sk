# playtime.sk

every second:
	loop all players:
		add 1 to {second::%loop-player%}
		if {second::%loop-player%} is 60:
			subtract 60 from {second::%loop-player%}
			add 1 to {minute::%loop-player%}
		if {minute::%loop-player%} is 60:
			subtract 60 from {minute::%loop-player%}
			add 1 to {hour::%loop-player%}
			
command /{@command} [<offline player>]:
	permission: {@permission}
	trigger:
		if arg 1 is not set:
			send "{@prefix} &7Your play time is %{hour::%player%}% hours %{minute::%player%}% minutes"
			stop
		if arg 1 is set:
			if arg 1 is "top":
				loop {hour::*}:
					add 1 to {_size}
					if {_low.to.high.list::%loop-value%} is not set:
						set {_low.to.high.list::%loop-value%} to loop-index
					else:
						set {_n} to 0
						loop {_size} times:
							set {_n} to {_n}+1
							{_low.to.high.list::%loop-value-1%.%{_n}%} is not set
							set {_low.to.high.list::%loop-value-1%.%{_n}%} to loop-index
							stop loop
				wait 1 tick
				set {_n} to size of {_low.to.high.list::*}
				loop {_low.to.high.list::*}:
					set {_high.to.low.list::%{_n}%} to loop-value
					set {_n} to {_n}-1
				wait 1 tick
				message "&m------------&r &6&lPlaytime &nTop 10&r &m------------"
				loop {_high.to.low.list::*}:
					add 1 to {_result}
					send "&b%loop-value%&7: &e%{hour::%loop-value%}% hours"
					if {_result} is 10:
						stop
			else:
				if {days.%arg 1%} is not set:
					set {days.%arg 1%} to 0
				if {hour::%arg 1%} is not set:
					set {hour::%arg 1%} to 0
				if {minute::%arg 1%} is not set:
					set {minute::%arg 1%} to 0
				send "{@prefix} &e%arg 1%&7's play time is %{hour::%arg 1%}% hours %{minute::%arg 1%}% minutes"
				stop
