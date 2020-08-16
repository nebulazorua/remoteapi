local mt = getrawmetatable(game)
setreadonly(mt, false)
local nc = mt.__namecall
local intercepted={}
local disabled = {}
local returns={}

local namecallMethod = getnamecallmethod or get_namecall_method

local methods={
	RemoteEvent='FireServer';
	RemoteFunction='InvokeServer';
}

getgenv().InterceptRemoteArgs=function(eventorfunc,args)
	intercepted[eventorfunc]=args
end

getgenv().SpoofReturn=function(func,newReturn)
	returns[func]=newReturn
end

getgenv().DisableRemote=function(eventorfunc)
	disabled[eventorfunc]=true;
end

getgenv().EnableRemote=function(eventorfunc)
	disabled[eventorfunc]=false
end

mt.__namecall=newcclosure(function(self,...)
	local Args = {...}
	local event = namecallMethod()
	if(methods[self.ClassName]==event)then
		if(disabled[self])then
			return;
		elseif(intercepted[self])then
			local intercept = intercepted[self]
			if(typeof(intercept)=='function')then
				Args={intercept(unpack(Args))}
			elseif(typeof(intercept)=='table')then
				Args=intercept
			end
		end
	end
	local returned={nc(self,unpack(Args))}
	if(returns[self] and returned)then
		returned={returns[self](unpack(returned))}
	end
	return unpack(returned)
end)
